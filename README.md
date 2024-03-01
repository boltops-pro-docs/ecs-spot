<!-- note marker start -->
NOTE: This repo contains only the documentation for the private BoltsOps repo code.
Original file: https://github.com/boltops-pro/ecs-spot/blob/master/README.md
The docs are publish so they are available for interested subscribers.
For access to the source code, you can become a BoltOps subscriber.
See: https://learn.boltops.com

<!-- note marker end -->

# ECS Spot Blueprint

[![Watch the video](https://img.boltops.com/boltopspro/video-preview/multiple/ecs-spot.png)](https://youtu.be/QzcgKac9m54)

![CodeBuild](https://codebuild.us-west-2.amazonaws.com/badges?uuid=eyJlbmNyeXB0ZWREYXRhIjoiVzRRcUp4dmt4UWZNb0tXSGxEaUxOZ012SHhkL3cyRzc5c0RwSEhVdmdDZTZzNHlUUU1iVHVPcDQxZitWT2pTUWlXRkZZQ25MaFVXUFZYMnhMUWpJWWVNPSIsIml2UGFyYW1ldGVyU3BlYyI6ImhQcEJpcXQ5YUtBNjhocmkiLCJtYXRlcmlhbFNldFNlcmlhbCI6MX0%3D&branch=master)

[![BoltOps Badge](https://img.boltops.com/boltops/badges/boltops-badge.png)](https://www.boltops.com)

This blueprint provision an AutoScaling spot-fleet cluster that registers instances to an ECS cluster.

* It contains a script that listens to the Spot two minute warning signal and calls [ECS Container Instance Draining](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/container-instance-draining.html).
* Contains [Auto Scaling alarms](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/cloudwatch_alarm_autoscaling.html) based on [ECS Cluster CPU and Memory Metrics](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/cloudwatch-metrics.html). So the on-demand fleet will grow as required to handle more containers. It will also scale down according to save costs.

## Prerequisite

* Setup config/settings.yml: [Settings Setup](https://github.com/boltopspro/reference-architecture/blob/master/docs/settings-setup.md)
* Create Service Roles: [ECS IAM Service Roles](docs/iam-service-roles.md)
* Create ECS Cluster with the CLI: [ECS Cluster](docs/ecs-cluster.md)

## Usage

1. Add blueprint to Gemfile
2. Configure: configs/ecs-spot values
3. Deploy blueprint

## Add

Add the blueprint to your lono project's `Gemfile`.

```ruby
gem "ecs-spot", git: "git@github.com:boltopspro/ecs-spot.git"
```

## Configure

Use the [lono seed](https://lono.cloud/reference/lono-seed/) command to generate a starter config params files.

    LONO_ENV=development lono seed ecs-spot
    LONO_ENV=production  lono seed ecs-spot

The files in `config/ecs-spot` folder will look something like this:

    configs/ecs-spot/
    ├── params
    │   ├── development.txt
    │   └── production.txt
    └── variables
        ├── development.rb
        └── production.rb

Configure the `configs/ecs-spot/params` and `configs/ecs-spot/variables` files.  There are 2 parameters required: `Subnets` and `Vpc`.

configs/ecs-spot/params/development.txt:

    # Required parameters:
    Subnets=subnet-111,subnet-222 # Find at vpc CloudFormation Outputs
    VpcId=vpc-111 # Find at vpc CloudFormation Outputs
    # Optional parameters:
    # EcsCluster=development
    # KeyName=...
    # MaxCapacity=100
    # MinCapacity=16
    # SnsTopicArn=...
    # TargetCapacity=16
    # ExistingSecurityGroup=...

configs/ecs-spot/variables/development.rb:

```ruby
@instance_types = {
  "m5.large" => {ram: 8.0, cpu: 2}, # supports ENI trunking
#  "r3.large" => {ram: 15.25, cpu: 2},
}
```

A quick way to get the VPC and subnet values is from the vpc CloudFormation Outputs. Here's an example of development.

![](https://img.boltops.com/boltopspro/blueprints/vpc/dev-vpc-outputs.png)

It is recommended to run the ECS containers on the `PrivateAppSubnets`.

Repeat the same process and configure params and variables files for the `production` environment also.

## Deploy

Use the [lono cfn deploy](http://lono.cloud/reference/lono-cfn-deploy/) command to deploy.

    LONO_ENV=development lono cfn deploy ecs-spot --sure --no-wait
    LONO_ENV=production  lono cfn deploy ecs-spot --sure --no-wait

If you are using One AWS Account, use these commands instead: [One Account](docs/one-account.md).

## Instance Access

To access the ec2 instances for debugging we strongly recommend using SSM Session Manager.  The instances in this blueprint have SSM manager installed.  You can quickly setup Session Manager on your AWS account with the [Session Manager Pro script](https://github.com/boltopspro/session-manager). There are also other ways to access your instance, more info: [Instance Access](docs/instance-access.md)

### Managed Security Group Rules

To open security group rules on the Managed Security Group you can use the `@security_group_ingress` variable. Example:

configs/ec2/variables/development.rb:

```ruby
@security_group_ingress = [{
  CidrIp: "0.0.0.0/0",
  FromPort: 22,
  IpProtocol: "tcp",
  ToPort: 22,
}]
```

### Custom UserData Script

The UserData can be customized with the `@user_data_script` variable.  The variable should be set to the path of the script. Example:

configs/ec2/variables/development.rb:

```ruby
@user_data_script = "configs/ec2/user_data/bootstrap.sh"
```

The script is wrapped in a [base64](https://lono.cloud/docs/intrinsic-functions/base64/) and [sub](https://lono.cloud/docs/intrinsic-functions/sub/) call. So [Pseudo Parameters](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/pseudo-parameter-reference.html) are available to be used in the script if needed. Example:

configs/ec2/user_data/bootstrap.sh

    echo ${AWS::StackName}

The custom `@user_data_script` is appended to an existing default UserData script that ships with the blueprint. The UserData runs cfn-init and applies configsets before the custom `@user_data_script`.

## IAM Permissions

The IAM permissions required for this stack are described below.

Service | Description
--- | ---
application-autoscaling | Spot Fleet autoscaling
cloudformation | To launch the CloudFormation stack.
cloudwatch | CloudWatch alarms for AutoScaling.
ec2 | Spot Fleet and Security groups.
iam | IAM roles for Instance, Spot Fleet, and Auto Scaling
s3 | Lono managed s3 bucket

## Considerations

This template listens to the [spot two-minute warning signal](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-interruptions.html#spot-instance-termination-notices) and calls [ECS Draining](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/container-instance-draining.html) when it is detected. ECS Draining essentially moves the Docker containers to another available EC2 Container Instances.

It takes some time for the new ECS task to reach steady state. If your Docker container bootup process takes too long, then this can result in unavailability. The availability detection is also dependent on the ELB Health Check settings.  If the ELB Target Group Health Check takes more than 2 minutes, then the new ECS task will not register in time.

**Summary:**

* Your application bootup needs to be fast enough.
* The ELB health check setting needs to be fast enough.
* Scaling up and down spot fleets will result in the two-minute warning being fired.
* Updating the ECS spot fleet CloudFormation stack does not fire the two-minute warning.
* Follow a best practice and always run at least 2 containers.

## Back to Reference Architecture

That's it. Go back to the main [boltopspro/reference-architecture](https://github.com/boltopspro/reference-architecture/blob/master/README.md)

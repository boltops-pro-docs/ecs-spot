<!-- note marker start -->
NOTE: This repo contains only the documentation for the private BoltsOps repo code.
Original file: https://github.com/boltops-pro/ecs-spot/blob/master/docs/instance-access.md
The docs are publish so they are available for interested subscribers.
For access to the source code, you can become a BoltOps subscriber.
See: https://learn.boltops.com

<!-- note marker end -->

# Instance Access

## SSM Session Manager

To access the ec2 instances for debugging we strongly recommend using SSM Session Manager.  The instances in this blueprint have SSM manager installed.  You can quickly setup Session Manager on your AWS account with the [Session Manager Pro script](https://github.com/boltopspro/session-manager).

Session Manager also supports ssh access without the need for a bastion or a opening up security group ports. See: [Enable SSH Connections Through Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-getting-started-enable-ssh-connections.html).

## SSH Access

You can also access the instance with traditional SSH if you prefer. To do so you'll need to configure stack parameters. We'll over them next.

## Security Groups

A security group will be assigned to the EC2 spot instances. The security group that may be created depends on how you configure the parameters.  Here's how they work:

* If you would like to use a pre-created existing security group then set the `ExistingSecurityGroups` parameter.  The existing security group will be used and no "managed" security group will be created by the CloudFormation template.
* If you would like the CloudFormation template to create and manage the security group for you. You can set the `SshLocation` parameter. A security group with Port 22 will be whitelisted to that `SshLocation`.
* If the `SshLocation` parameter is not set, then no managed security group will be created and the default security group will be assigned to the EC2 instances in the AutoScaling group.

## Keypair

Though it is optional, it is useful to configure an ssh keypair for the stack in case you need to ssh into the instances it creates and debug.  The params files to configure this is under `configs/ecs-spot/params`. For example:

configs/ecs-spot/params/development.txt:

```
KeyName=default-2018-08-08 # change this to an ssh keyname that exists on your account
```

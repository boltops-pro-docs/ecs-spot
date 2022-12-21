<!-- note marker start -->
**NOTE**: This repo contains only the documentation for the private BoltsOps Pro repo code.
Original file: https://github.com/boltopspro/ecs-spot/blob/master/docs/iam-service-roles.md
The docs are publish so they are available for interested customers.
For access to the source code, you must be a paying BoltOps Pro subscriber.
If are interested, you can contact us at contact@boltops.com or https://www.boltops.com

<!-- note marker end -->

## ECS IAM Service Roles

The ecs-spot blueprint uses some IAM service roles. Here are the commands to create them:

    aws iam create-service-linked-role --aws-service-name spotfleet.amazonaws.com
    aws iam create-service-linked-role --aws-service-name spot.amazonaws.com

More info about IAM Service roles: [Using Service-Linked Roles
](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html)

Note, if you are using [lono seed](https://lono.cloud/reference/lono-seed/), then the logic will create these roles for you so you don't have to do it manually.

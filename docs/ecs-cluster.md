<!-- note marker start -->
NOTE: This repo contains only the documentation for the private BoltsOps repo code.
Original file: https://github.com/boltops-pro/ecs-spot/blob/master/docs/ecs-cluster.md
The docs are publish so they are available for interested subscribers.
For access to the source code, you can become a BoltOps subscriber.
See: https://learn.boltops.com

<!-- note marker end -->

## Create ECS Cluster Commands

    AWS_PROFILE=dev aws ecs create-cluster --cluster-name development
    AWS_PROFILE=prd aws ecs create-cluster --cluster-name production

We create the clusters with the CLI instead of the ECS Console wizard because we don't want the ECS cluster to be coupled to the network that is created.

If you are using One AWS Account, use these commands instead: [One Account](docs/one-account.md).  Use these commands instead:

    AWS_PROFILE=one aws ecs create-cluster --cluster-name development
    AWS_PROFILE=one aws ecs create-cluster --cluster-name production

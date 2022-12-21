<!-- note marker start -->
**NOTE**: This repo contains only the documentation for the private BoltsOps Pro repo code.
Original file: https://github.com/boltopspro/ecs-spot/blob/master/docs/ecs-cluster.md
The docs are publish so they are available for interested customers.
For access to the source code, you must be a paying BoltOps Pro subscriber.
If are interested, you can contact us at contact@boltops.com or https://www.boltops.com

<!-- note marker end -->

## Create ECS Cluster Commands

    AWS_PROFILE=dev aws ecs create-cluster --cluster-name development
    AWS_PROFILE=prd aws ecs create-cluster --cluster-name production

We create the clusters with the CLI instead of the ECS Console wizard because we don't want the ECS cluster to be coupled to the network that is created.

If you are using One AWS Account, use these commands instead: [One Account](docs/one-account.md).  Use these commands instead:

    AWS_PROFILE=one aws ecs create-cluster --cluster-name development
    AWS_PROFILE=one aws ecs create-cluster --cluster-name production

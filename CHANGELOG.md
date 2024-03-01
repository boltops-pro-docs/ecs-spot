<!-- note marker start -->
NOTE: This repo contains only the documentation for the private BoltsOps repo code.
Original file: https://github.com/boltops-pro/ecs-spot/blob/master/CHANGELOG.md
The docs are publish so they are available for interested subscribers.
For access to the source code, you can become a BoltOps subscriber.
See: https://learn.boltops.com

<!-- note marker end -->

# Change Log

All notable changes to this project will be documented in this file.
This project *loosely tries* to adhere to [Semantic Versioning](http://semver.org/).

## [2.2.2]
- #5 allow alarm thresholds to be configured with params

## [2.2.1]
- update PolicyName to spot-terminating-handler

## [2.2.0]
- change variable to `@iam_policies_override`

## [2.1.4]
- update ECS AMIs

## [2.1.3]
- allow extra iam permissions that decorate: `@iam_managed_policy_arns_extra` and `@iam_policies_extra`

## [2.1.2]
- update ECS AMIs

## [2.1.1]
- update ECS AMIs

## [2.1.0]
- #4 allow IAM permission customizations
- update ECS AMIs

## [2.0.0]
- #3 upgrade to lono v7
- upgrade ecs AMIs
- configsets: cfn-hup, ssm, awslogs
- add CloudWatch logs IAM permission
- allow custom UserScript with variable: @user_data_script
- customization of security group rules with variable: @security_group_ingress
- set tags as variable: @tags
- use parameter_group
- add variables_helper

## [1.0.2]
- cleanup: ref Conditional true

## [1.0.1]
- update ECS ami

## [1.0.0]
- #2 lono v6 upgrade: auto_camelize false
- rename parameter to VpcId
- use `create_service_linked_role` helper in seed file

## [0.5.5]
- fix FleetRole and remove custom iam policy

## [0.5.4]
- fix autoscaling typo

## [0.5.3]
- docs: comments to starter parameter values
- docs: docs/one-account.md

## [0.5.2]
- update ecs amis
- get_att camelize name

## [0.5.1]
- cleanup: remove old scripts

## [0.5.0]
- #1 upgrade to lono v5.2 and use camelize naming
- upgrade seeds/config.rb

## [0.4.0]
- update ecs amis
- fix 2m warning ecs draining
- update setup configs

## [0.3.0]
- fix ec2 tagging on new instances: iam permissions

## [0.2.0]
- rename security group default

## [0.1.0]
- Initial release

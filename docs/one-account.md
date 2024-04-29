<!-- note marker start -->
NOTE: This repo contains only the documentation for the private BoltsOps repo code.
Original file: https://github.com/boltops-pro/ecs-spot/blob/master/docs/one-account.md
The docs are publish so they are available for interested subscribers.
For access to the source code, you can become a BoltOps subscriber.
See: https://learn.boltops.com

<!-- note marker end -->

## One AWS Account Commands

[![Watch the video](https://img.boltops.com/boltopspro/video-preview/single/ecs-spot.png)](https://www.youtube.com/watch?v=l2Gy7vlflfw)

Here are the summarized commands if you're using the One AWS account strategy.

## Deploy

Let's deploy now with [lono deploy](https://lono.cloud/reference/lono-cfn-deploy/).

    LONO_ENV=development lono cfn deploy ecs-spot-development --blueprint ecs-spot --sure --no-wait
    LONO_ENV=production  lono cfn deploy ecs-spot-production  --blueprint ecs-spot --sure --no-wait

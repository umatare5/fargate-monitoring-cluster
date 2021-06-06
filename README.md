# fargate-monitoring-cluster

An example to manage multi-container applications using [copilot](https://github.com/aws/copilot-cli).

## Background

`copilot` is an awesome CLI to manage container applications on AWS!

But it's not supported `docker-compose.yml`. I hope to manage multi-containers all in one.

## To Do

- Watch availability of [my website](https://umatare5.netlify.app) using Prometheus.

## Architecture

### Overview

```bash
*== Account: ID ============================================*
|                                                           |
| +-- Region: ap-northeast-1 --------------------------+    |    +~~~~~~ Internet ~~~~~~+
| |                                                    |    |    |                      |
| | +-- Fargate -----------------------------------+   |    |    |       +---------+    |
| | |                                              |   |    |    |  +----+ Browser |    |
| | | +-- Cluster: monitoring -----------------+   |   |    |    |  |    +---------+    |
| | | |                                        |   |   |    |    |  |                   |
| | | |  service: prometheus        [c]<---------------[ LB ]<------+    +---------+    |
| | | |  service: alertmanager      [c]--------------------------------->|  Slack  |    |
| | | |  service: blackbox-expoter  [c]-----------------------------+    +---------+    |
| | | |                                        |   |   |    |    |  |                   |
| | | +----------------------------------------+   |   |    |    |  |    +---------+    |
| | +----------------------------------------------+   |    |    |  +--->| Netlify |    |
| |          +----------------+ +-----+ +---------+    |    |    |       +---------+    |
| |          | System Manager | | VPC | | Route53 |    |    |    |                      |
| |          +----------------+ +-----+ +---------+    |    |    +~~~~~~~~~~~~~~~~~~~~~~+
| +----------------------------------------------------+    |
|                     +---------------+ +---------+         |
|                     | Cert. Manager | | Route53 |         |
|                     +---------------+ +---------+         |
*===========================================================*
```

### Description

- This repository deploys `monitoring` cluster.
- The cluster contains Prometheus, Alertmanager and blackbox-exporter.
- Prometheus is associated with LB with source IP restrictions and faces the internet.
- LB has dedicated hostname in my domain. The name register to Route53 automatically.
- LB terminates SSL connection by using my domain's certificate.
- Alertmanager and blackbox-exporter are backend services.
- They access to internet when requested from Prometheus.

### Directory Structure

| Name          | Description                                                                    |
| :------------ | ------------------------------------------------------------------------------ |
| Dockerfile.\* | Dockerfiles for services that be deploying.                                    |
| /services     | Container application directory. It contains settings for containers.          |
| /copilot      | `copilot` managed directory. It contains settings for Fargate, ELB and others. |
| /docs         | Documents directory. It contains guides that supplements the `README.md`.      |

## Setup

Refer to [SETUP_GUIDE.md](./docs/SETUP_GUIDE.md).

## Deployment

Refer to [DEPLOYMENT_GUIDE.md](./docs/DEPLOYMENT_GUIDE.md).

## Closing

Refer to [CLOSING_GUIDE.md](./docs/CLOSING_GUIDE.md).

## Troubleshooting

Refer to [TROUBLESHOOTING.md](./docs/TROUBLESHOOTING.md).

## Note

- Add reverse proxy.

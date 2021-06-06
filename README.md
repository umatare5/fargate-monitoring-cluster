# fargate-monitoring-cluster

An example structure with individual Dockerfiles using `copilot`.

## Background

Amazon Fargate is not supported `docker-compose.yml`.

But I hope to manage multi-container applications in monolithic repository.

## To Do

- Watch availability of [my website](https://umatare5.netlify.app) using Prometheus.

## Description

- This repository manages a `monitoring` cluster.
- The cluster includes Prometheus, Alertmanager and blackbox-exporter.
- Prometheus is associated with LB with source IP restrictions and faces the internet.
- LB has dedicated hostname in my domain. The name register to Route53 automatically.
- LB terminates SSL connection by using my domain's certificate.
- Alertmanager and blackbox-exporter are backend services.
- They access to internet when requested from Prometheus.

## Architecture

### Overview

```bash
*== Account: ID ============================================*
|                                                           |
| +-- Region: ap-northeast-1 --------------------------+    |    +~~~~~~ Internet ~~~~~~+
| |                                                    |    |    |                      |
| | +-- Fargate -----------------------------------+   |    |    |      +---------+     |
| | |                                              |   |    |    |  +---+ Browser |     |
| | | +-- Cluster: monitoring -----------------+   |   |    |    |  |   +---------+     |
| | | |                                        |   |   |    |    |  |                   |
| | | |  service: prometheus        [c]<---------------[ LB ]<------+   +---------+     |
| | | |  service: alertmanager      [c]-------------------------------->|  Slack  |     |
| | | |  service: blackbox-expoter  [c]-----------------------------+   +---------+     |
| | | |                                        |   |   |    |    |  |                   |
| | | +----------------------------------------+   |   |    |    |  |   +---------+     |
| | +----------------------------------------------+   |    |    |  +-->| Netlify |     |
| |          +----------------+ +-----+ +---------+    |    |    |      +---------+     |
| |          | System Manager | | VPC | | Route53 |    |    |    |                      |
| |          +----------------+ +-----+ +---------+    |    |    +~~~~~~~~~~~~~~~~~~~~~~+
| +----------------------------------------------------+    |
|                     +---------------+ +---------+         |
|                     | Cert. Manager | | Route53 |         |
|                     +---------------+ +---------+         |
*===========================================================*
```

```bash
$ copilot svc ls
Name                Type
----                ----
alertmanager        Backend Service
blackbox-exporter   Backend Service
prometheus          Load Balanced Web Service
```

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

# Setup Guide

## Quickstart

```bash
copilot app init --domain estab.dev                  # CloudFormation: monitoring-infrastructure-roles
copilot env init --name z1 --profile default --app monitoring --prod # CloudFormation: monitoring-z1
copilot svc init --name blackbox-exporter            # CloudFormation: None
copilot svc init --name alertmanager                 # CloudFormation: None
copilot svc init --name prometheus                   # CloudFormation: None
copilot svc deploy --name blackbox-exporter --env z1 # CloudFormation: monitoring-z1-blackbox-exporter
copilot svc deploy --name blackbox-exporter-with-eip --env z1 # CloudFormation: monitoring-z1-blackbox-exporter-with-eip
copilot svc deploy --name alertmanager --env z1      # CloudFormation: monitoring-z1-alertmanager
copilot svc deploy --name prometheus --env z1        # CloudFormation: monitoring-z1-alertmanager
```

## Step-by-Step

- Initialize application.

  ```console
  $ copilot app init --domain estab.dev
  Application name: monitoring
  ✔ Created the infrastructure to manage services and jobs under application monitoring.

  ✔ The directory copilot will hold service manifests for application monitoring.

  Recommended follow-up actions:
  - Run `copilot init` to add a new service or job to your application.
  ```

- Initialize environment. "z" means "production", "1" means first environment.

  ```console
  $ copilot env init --name z1 --profile default --app monitoring --prod
  Would you like to use the default configuration for a new environment?
      - A new VPC with 2 AZs, 2 public subnets and 2 private subnets
      - A new ECS Cluster
      - New IAM Roles to manage services and jobs in your environment
   Yes, use default.
  ✔ Linked account 123456789000 and region ap-northeast-1 to application monitoring.

  ✔ Proposing infrastructure changes for the monitoring-z1 environment.
  - Creating the infrastructure for the monitoring-z1 environment.          [create complete]  [141.2s]
    - An IAM Role for AWS CloudFormation to manage resources                [create complete]  [18.6s]
    - An ECS cluster to group your services                                 [create complete]  [9.5s]
    - Delegate DNS for environment subdomain                                [create complete]  [39.5s]
    - Enable long ARN formats for the authenticated AWS principal           [create complete]  [0.0s]
    - An IAM Role to describe resources in your environment                 [create complete]  [22.3s]
    - A security group to allow your containers to talk to each other       [create complete]  [3.8s]
    - Request and validate an ACM certificate for your domain               [create complete]  [45.9s]
    - An Internet Gateway to connect to the public internet                 [create complete]  [16.4s]
    - Private subnet 1 for resources with no internet access                [create complete]  [16.2s]
    - Private subnet 2 for resources with no internet access                [create complete]  [16.2s]
    - Public subnet 1 for resources that can access the internet            [create complete]  [16.2s]
    - Public subnet 2 for resources that can access the internet            [create complete]  [16.2s]
    - A Virtual Private Cloud to control networking of your AWS resources   [create complete]  [16.4s]
  ✔ Created environment z1 in region ap-northeast-1 under application monitoring.
  ```

- Initialize secrets.

  ```console
  $ copilot secret init
  secret name:  SLACK_WEBHOOK_URL
  What is the value of secret SLACK_WEBHOOK_URL in environment production?
  Environment production is already on the latest version v1.4.0, skip upgrade.
  ...Put secret SLACK_WEBHOOK_URL to environment production
  ✔ Successfully put secret SLACK_WEBHOOK_URL in environment production as /copilot/monitoring/production/secrets/SLACK_WEBHOOK_URL.
  You can refer to these secrets from your manifest file by editing the `secrets` section.
  production
    secrets:
      SLACK_WEBHOOK_URL: /copilot/monitoring/production/secrets/SLACK_WEBHOOK_URL
  ```

- Initialize services.

  ```console
  $ copilot svc init --name blackbox-exporter
  Note: It's best to run this command in the root of your workspace.
  Service type: Backend Service
  Dockerfile: ./Dockerfile.blackbox-exporter
  ✔ Manifest file for service blackbox-exporter already exists at copilot/blackbox-exporter/manifest.yml, skipping writing it.
  Your manifest contains configurations like your container size and port (:9115).

  ✔ Created ECR repositories for service blackbox-exporter.
  ```

  ```console
  $ copilot svc init --name alertmanager
  Note: It's best to run this command in the root of your workspace.
  Service type: Backend Service
  Dockerfile: ./Dockerfile.alertmanager
  ✔ Manifest file for service alertmanager already exists at copilot/alertmanager/manifest.yml, skipping writing it.
  Your manifest contains configurations like your container size and port (:9093).

  ✔ Created ECR repositories for service alertmanager.

  Recommended follow-up actions:
  - Update your manifest copilot/alertmanager/manifest.yml to change the defaults.
  - Run `copilot svc deploy --name alertmanager --env test` to deploy your service to a test environment.
  ```

  ```console
  $ copilot svc init --name prometheus
  Note: It's best to run this command in the root of your workspace.
  Service type: Load Balanced Web Service
  Dockerfile: ./Dockerfile.prometheus
  ✔ Wrote the manifest for service prometheus at copilot/prometheus/manifest.yml
  Your manifest contains configurations like your container size and port (:9090).

  ✔ Created ECR repositories for service prometheus.

  Recommended follow-up actions:
  - Update your manifest copilot/prometheus/manifest.yml to change the defaults.
  - Run `copilot svc deploy --name prometheus --env test` to deploy your service to a test environment.
  ```

- Deploy services.

  Refer to [DEPLOYMENT_GUIDE.md](./DEPLOYMENT_GUIDE.md).

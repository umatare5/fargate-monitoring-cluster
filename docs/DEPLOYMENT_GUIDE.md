# Deployment Guide

## Quickstart

```bash
copilot svc deploy --name alertmanager --env z1
copilot svc deploy --name blackbox-exporter --env z1
copilot svc deploy --name prometheus --env z1
```

## Step-by-Step

- Deploy alertmanager.

  ```bash
  copilot svc deploy --name alertmanager --env z1
  ```

- Deploy blackbox-exporter.

  ```bash
  copilot svc deploy --name blackbox-exporter --env z1
  ```

- Deploy prometheus.

  ```bash
  copilot svc deploy --name prometheus --env z1
  ```

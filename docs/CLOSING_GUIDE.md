# Closing Guide

## Quickstart

```bash
copilot svc delete --name prometheus --env production
copilot svc delete --name alertmanager --env production
copilot svc delete --name blackbox-exporter --env production
copilot app delete --name monitoring
copilot env delete --name z1
```

## Step-by-Step

- Delete prometheus.

  ```bash
  copilot svc delete --name prometheus --env production
  ```

- Delete alertmanager.

  ```bash
  copilot svc delete --name alertmanager --env production
  ```

- Delete blackbox-exporter.

  ```bash
  copilot svc delete --name blackbox-exporter --env production
  ```

- Delete application.

  ```bash
  copilot app delete --name monitoring
  ```

- Delete environment.

  ```bash
  copilot env delete --name z1
  ```

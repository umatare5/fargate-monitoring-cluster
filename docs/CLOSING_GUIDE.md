# Closing Guide

## Quickstart

```bash
copilot svc delete --name prometheus --env z1
copilot svc delete --name alertmanager --env z1
copilot svc delete --name blackbox-exporter --env z1
copilot app delete --name monitoring
copilot env delete --name z1
```

## Step-by-Step

- Delete prometheus.

  ```bash
  copilot svc delete --name prometheus --env z1
  ```

- Delete alertmanager.

  ```bash
  copilot svc delete --name alertmanager --env z1
  ```

- Delete blackbox-exporter.

  ```bash
  copilot svc delete --name blackbox-exporter --env z1
  ```

- Delete application.

  ```bash
  copilot app delete --name monitoring
  ```

- Delete environment.

  ```bash
  copilot env delete --name z1
  ```

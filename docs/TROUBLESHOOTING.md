# Troubleshooting Guide

## Container debugging

- Quickstart

  ```fish
  export SVC_NAME=hooks.slack.com/services/hoo/bar/baz
  export SLACK_WEBHOOK_URL=hooks.slack.com/services/hoo/bar/baz
  docker container stop $SVC_NAME
  docker container rm $SVC_NAME
  docker image build -f Dockerfile.$SVC_NAME -t local/$SVC_NAME:dev .
  docker container create --env SLACK_WEBHOOK_URL=$SLACK_WEBHOOK_URL --name $SVC_NAME local/$SVC_NAME:dev
  docker container start  $SVC_NAME
  docker container logs   $SVC_NAME
  ```

- Set `SVC_NAME`. (Able to choose `prometheus` or `blackbox-exporter`)

  ```bash
  export SVC_NAME=alertmanager
  ```

- Build the image.

  ```bash
  docker image build -f Dockerfile.$SVC_NAME -t local/$SVC_NAME:dev .
  ```

  ```bash
  $ docker image ls local/$SVC_NAME:dev
  REPOSITORY           TAG       IMAGE ID       CREATED          SIZE
  local/alertmanager   dev       4a1548aa6094   15 minutes ago   51.6MB
  ```

- Register to boot a container from the image. (These commands are same as `docker run`)

  ```bash
  docker container create --env SLACK_WEBHOOK_URL=$SLACK_WEBHOOK_URL --name $SVC_NAME local/$SVC_NAME:dev
  docker container start $SVC_NAME
  ```

  - When deploy Prometheus, you need to set `REMOTE_STORAGE_URL`, `REMOTE_STORAGE_USERNAME` and `REMOTE_STORAGE_PASSWORD`.

- Show boot logs.

  ```bash
  docker container logs $SVC_NAME
  ```

- Use console.

  ```bash
  docker container exec -it $SVC_NAME /bin/sh
  ```

- Remove the container.

  ```bash
  docker container stop $SVC_NAME
  docker container rm $SVC_NAME
  ```

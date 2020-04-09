# Dockerized REACT

## How to use it via Docker Compose

build the image and fire up the container

```bash
docker-compose up -d --build
```

Run on:  <http://localhost:3001/>
Can do change in App.js to see the reload

## How to use it via Dockerfile

Build and tag the Docker image:

```bash
 docker build -t jdh:dev .
```

Spin up the container

```bash
docker run \
    -it \
    --rm \
    -v ${PWD}:/app \
    -v /app/node_modules \
    -p 3001:3000 \
    -e CHOKIDAR_USEPOLLING=true \
    jdh:dev
```

Explanation of the parameter:

|  docker run                  |                                                           |
|------------------------------|-----------------------------------------------------------|
|  -it                         |                                                           |
|  -rm                         |                                                           |
|  -v ${PWD}:/app              |                                                           |
|  -v /app/node_modules        |                                                           |
|  -p 3001:3000                |                                                           |
|  -e CHOKIDAR_USEPOLLING=true |    hot-reloading will work                                |

## Explanation of the Dockerfile

```bash
FROM node:13.12.0-alpine
```

Alpine Linux is much smaller than most distribution base images (~5MB), and thus leads to much slimmer images in general.

Documentation:
<https://github.com/nodejs/docker-node/blob/master/README.md#how-to-use-this-image>

```bash
RUN npm install --silent
RUN npm install react-scripts@3.4.1 -g --silent
```

Silencing NPM
Addition of a .dockerignore, speed up the Docker build, the node_modules directory will not be sent to the Docker daemon.

## Explanation of the Docker-compose

```bash
    volumes:
      - '.:/app'
      - '/app/node_modules'
```

## Addition of one parameter due to bug

Addition of the flag:

```bash
stdin_open: true
```

<https://github.com/facebook/create-react-app/issues/8688>



## Skycoin Infrastructure
#### Docker images. Best practices

----------------

#### These slides: [slides.skycoin.net/skycoin.docker.html](http://slides.cuban.tech/skycoin.docker.html)

#### Authors : Olemis Lang , Ariel Lima , and the Skycoin community
###### powered by [reveal.js](https://revealjs.com/)

----------------

###### The Skycoin Platform is the most advanced blockchain platform in the world.

--

## Outline

- Docker best practices
- Multi-stage builds in Skycoin Dockerfile's
- Build `arm` images on `amd64` architectures. Hooks.
- Designing entry points for seamless command execution.
- Map container user IDs to host user IDs
- Virtualizing developer workspaces
- Wishlist
  * Multi-base images
  * OS base images without package manager

---

## Outline
#### Virtual workspaces

- Development base image
- Docker development inside Docker containers
- Support multiple languages (PySkycoin, .NET). Client libraries.
- Run your favorite IDE inside Docker containers (VS Code example)
- Eclipse Che
  * What is it?
  * What are Docker containers used for?
- Codenvy

--

## Skycoin Docker images
#### Use build arguments

Mainly for:

- Base image
  * Switch smoothly from one OS to the other
  * Upgrades in programming language tools needed by your app
- Software version, build info
- Dockerfile command arguments
  * Golang `ARCH`

---

## Skycoin Docker images 
#### Use build arguments

![Build args in skycoin/skycoin image](img/docker.skycoin.buildargs.png)

--

## Skycoin Docker images
#### Build for multiple architectures

- Project goal: run on Raspberry Pi , Orange Pi and alike
- Publish as image tags. Use build hooks.

---

## Skycoin Docker images
#### skycoin/skycoin [build hook](https://github.com/skycoin/skycoin/tree/develop/docker/images/mainnet/hooks/build)

```
#!/bin/bash
cd ../../../
docker build -f $DOCKERFILE_PATH -t $IMAGE_NAME .
docker build --build-arg=ARCH=arm --build-arg=GOARM=5 --build-arg=IMAGE_FROM="arm32v5/busybox" -f $DOCKERFILE_PATH -t $IMAGE_NAME-arm32v5 .
docker build --build-arg=ARCH=arm --build-arg=GOARM=6 --build-arg=IMAGE_FROM="arm32v6/busybox" -f $DOCKERFILE_PATH -t $IMAGE_NAME-arm32v6 .
docker build --build-arg=ARCH=arm --build-arg=GOARM=7 --build-arg=IMAGE_FROM="arm32v7/busybox" -f $DOCKERFILE_PATH -t $IMAGE_NAME-arm32v7 .
docker build --build-arg=ARCH=arm64 --build-arg=IMAGE_FROM="arm64v8/busybox" -f $DOCKERFILE_PATH -t $IMAGE_NAME-arm64v8 .
```

---

## Skycoin Docker images
#### skycoin/skycoin [push hook](https://github.com/skycoin/skycoin/tree/develop/docker/images/mainnet/hooks/push)

```
#!/bin/bash
docker push $IMAGE_NAME
docker push $IMAGE_NAME-arm32v5
docker push $IMAGE_NAME-arm32v6
docker push $IMAGE_NAME-arm32v7
docker push $IMAGE_NAME-arm64v8
```

--

## Skycoin Docker images
#### Multi-stage builds

![Build with golang:1.11-stretch run busybox](img/docker.skycoin.imgbase.png)

--

## Skycoin Docker images
#### Multi-stage builds

Busybox? Why not Alpine ?

--

## Skycoin Docker images
#### Multi-stage builds

- No images for architectures `arm6` and `arm8`
  * ... since some moment in time during 2018
- Includes `apk` package manager
  * Software package set in production images should be frozen
    - ... i.e. nothing else needs to be installed
    - ... generally speaking there could be exceptions
  * [Vulnerabilities found](https://justi.cz/security/2018/09/13/alpine-apk-rce.html) recently (2018)
- `busybox` image works and is even smaller

--

## Docker base practices
#### Ephemeral containers

`/data` and `/wallet` volumes




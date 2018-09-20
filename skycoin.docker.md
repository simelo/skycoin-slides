

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
- Software version, build info (more on this later ...)
- Dockerfile command arguments
  * Golang `ARCH` (did I say later? ...)

---

## Skycoin Docker images 
#### Use build arguments

![Build args skycoin/skycoin image](img/docker.buildargs.png)

--

## Skycoin Docker images
#### Multi-stage builds


--

## Docker base practices
#### Ephemeral containers

`/data` and `/wallet` volumes




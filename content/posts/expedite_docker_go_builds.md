---
title: "Expedite Golang builds in Docker"
date: 2020-08-19
draft: false
toc: true
---

## The problem
I was recently working on a Golang API that imported the [Kubernetes client](https://github.com/kubernetes/client-go). I implemented [Skaffold](https://skaffold.dev/) to streamline the local development cycle, but rebuilds after source code changes consistently took **50 seconds**, no matter how small the change. That's unacceptably long, in my opinion, so I set about the task of improving the development cycle by optimizing the Docker build time.

## Downloading dependencies
After passing the `-v` flag to the `go build` command, I noticed that a significant length of time was spent fetching the dependencies. 

There are two ways to address this problem.

```Docker
COPY go.mod go.sum .
RUN go mod download
...
COPY . .
RUN go build .
```

In a nutshell, by copying `go.mod` and `go.sum` then running `go mod download`, you're consolidating the dependency retrieval into a standalone Docker image layer which gets cached. Subsequent rebuilds will leverage the cache, except in the comparatively rare exception that you make changes to `go.mod` or `go.sum`.

If you're vendoring, committing your `vendor` directory to source, and using Go v1.14 or greater, you can use the vendor directory instead of re-downloading your dependencies:

```Docker
COPY . . # Including the vendor directory
ENV GO111MODULE=on
RUN go build .
```

## Compilation
This improved my build time by about 10-15 seconds, but it was still unacceptably long. 

The problem was that my dependencies (and their dependencies) were pretty large in scope and took a long time to compile. 

## BuildKit to the rescue
I explored a few options, including compiling only the dependencies in a Docker image layer and mounting my GOCACHE to the build, but the solution turned out to be pleasantly simple. 

Enable [BuildKit](https://docs.docker.com/develop/develop-images/build_enhancements/) by running:

```bash
export DOCKER_BUILDKIT=1
```

and adding:

```
# syntax = docker/dockerfile:1-experimental
```

to the top of your Dockerfile, then preface your `go build` command in the Dockerfile as follows:

```
RUN --mount=type=cache,target=/root/.cache/go-build \
go build
```

## How it works
In a nutshell, the mount implements a cache that persists across multiple builds. After an initial, extended build, subsequent builds leverage the cache and are therefore much more efficient. 

This *dramatically* reduced my build time. 

Before enabling the cache:
```bash
❯ docker build .
[+] Building 47.8s (15/15) FINISHED 
```

After enabling the cache:
```bash
❯ docker build .
[+] Building 7.6s (15/15) FINISHED 
```

That's a **40.2** second reduction in build time. Not too shabby! 

Side note, I'm using a [multistage build](https://docs.docker.com/develop/develop-images/multistage-build/), but this advice is equally applicable regardless of stage count. 

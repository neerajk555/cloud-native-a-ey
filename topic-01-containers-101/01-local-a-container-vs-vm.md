# Topic 1 / Local A: Container vs. VM, and Your First Container

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Docker installed and running on your Ubuntu VM (`docker version` shows both Client and Server sections).

Understand what a container actually is by running one, before learning any other commands.

## Step 1: Run your first container

```
docker run hello-world
```

Read the output — it explains, in Docker's own words, exactly what just happened: pulling the image, creating a container, running it.


## Step 2: See the container that ran (and already exited)

```
docker ps -a
```

`-a` shows ALL containers including stopped ones. Without it, `docker ps` only shows currently running containers.


## Step 3: See the image that was downloaded

```
docker images
```

This is the read-only template — you can create as many containers from it as you like.


## Step 4: Run a full interactive container

```
docker run -it ubuntu bash
```

Type `ls`, look around, then `exit`. You just got a shell inside an isolated Ubuntu environment.


## Sanity checks

`docker ps -a` shows at least two containers (hello-world and your ubuntu session), both Exited.

Common places beginners get stuck: Expecting `docker ps` (no -a) to show a finished container (It won't — only running containers show without -a.) Thinking a container is a tiny VM (It's a different isolation mechanism — no separate kernel, no separate boot process, which is exactly why it's so much faster and lighter.)

## Cleanup

```
docker rm $(docker ps -aq)
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

# Topic 1 / Local B: Container Lifecycle: Run, Stop, Remove

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Local A complete.

Practice every stage of a container's life deliberately, so 'why did my container disappear' never confuses you again.

## Step 1: Run a long-lived container in the background

```
docker run -d --name lifecycle-demo nginx
```


## Step 2: Confirm it's running

```
docker ps
```


## Step 3: Stop it (graceful shutdown, not deleted)

```
docker stop lifecycle-demo
docker ps -a
```

Still listed — stopped, not gone.


## Step 4: Start it again

```
docker start lifecycle-demo
```

Same container, same filesystem state as before it stopped.


## Step 5: Remove it for good

```
docker stop lifecycle-demo
docker rm lifecycle-demo
```


## Sanity checks

`docker ps -a` no longer lists lifecycle-demo at all.

Common places beginners get stuck: Trying `docker rm` on a running container (You'll get an error — stop it first, or use `docker rm -f` to force both in one step.) Assuming `docker stop` deletes data (It doesn't — the container and its state persist until `docker rm`.)

## Cleanup

```
docker rm -f lifecycle-demo 2>/dev/null || true
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

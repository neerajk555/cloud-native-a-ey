# Topic 3 / Local A: Port Mapping Explained and Practiced

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Topic 1 complete.

Understand exactly how host ports connect to container ports, and see the failure mode when you forget it.

## Step 1: Run nginx WITHOUT port mapping

```
docker run -d --name no-port nginx
```

Try http://localhost — nothing responds. The container's network is isolated by default.


## Step 2: Run nginx WITH port mapping

```
docker run -d --name with-port -p 8080:80 nginx
```


## Step 3: Test it

```
curl http://localhost:8080
```


## Step 4: See the mapping explicitly

```
docker port with-port
```


## Sanity checks

- curl on :8080 returns nginx's welcome HTML; a fresh curl attempt on plain :80 fails.

Common places beginners get stuck: Mixing up host/container port order (It's always `-p HOST:CONTAINER` — 8080:80 means 'my machine's 8080 maps to the container's 80'.)

## Cleanup

```
docker rm -f no-port with-port
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

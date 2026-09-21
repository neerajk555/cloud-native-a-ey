# Topic 4 / Local A: Health Checks and Restart Policies

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Topic 1 complete.

Configure both, and see the difference between a container being 'running' and being 'healthy'.

## Step 1: Run with a health check

```
docker run -d --name health-demo --health-cmd='curl -f http://localhost/ || exit 1' --health-interval=10s --health-retries=3 nginx
```


## Step 2: Watch the status become healthy

```
sleep 12
docker inspect --format='{{.State.Health.Status}}' health-demo
```


## Step 3: Run something that crashes, with a restart policy

```
docker run -d --name restart-demo --restart=unless-stopped alpine sh -c 'sleep 5 && exit 1'
```


## Step 4: Watch it restart automatically

```
sleep 20
docker ps -a --filter name=restart-demo
```

Check the STATUS/RESTART COUNT — it should show it's restarted at least once.


## Sanity checks

- health-demo shows 'healthy'; restart-demo shows a restart count greater than 0.

Common places beginners get stuck: Assuming 'running' means 'working correctly' (Running only means the process hasn't exited — a frozen or broken app can still show as running without a health check to catch it.) Using --restart=always when you plan to `docker stop` it yourself sometimes (always restarts even after a manual stop; use unless-stopped instead for anything you might deliberately stop.)

## Cleanup

```
docker rm -f health-demo restart-demo
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

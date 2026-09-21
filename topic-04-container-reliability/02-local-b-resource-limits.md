# Topic 4 / Local B: Resource Limits

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Local A complete.

Cap CPU and memory so one container can't starve everything else, and see what happens when a limit is exceeded.

## Step 1: Run with limits

```
docker run -d --name limited --memory=128m --cpus=0.5 nginx
```


## Step 2: Confirm the limits took effect

```
docker inspect limited --format='Memory: {{.HostConfig.Memory}} bytes, NanoCPUs: {{.HostConfig.NanoCpus}}'
```


## Step 3: Watch live usage

```
docker stats limited --no-stream
```


## Step 4: Deliberately exceed a tiny memory limit

```
docker run --memory=10m --name oom-test python:3-slim python -c "x=[0]*10**8" || true
docker inspect oom-test --format='OOMKilled: {{.State.OOMKilled}}'
```


## Sanity checks

- The last command shows `OOMKilled: true` — proof the kernel terminated the container for exceeding its memory limit.

Common places beginners get stuck: Not setting any limits in a shared/training environment (One runaway container can degrade everything else on the same host — exactly why the AWS side of this course enforces instance-size limits too.)

## Cleanup

```
docker rm -f limited oom-test 2>/dev/null || true
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

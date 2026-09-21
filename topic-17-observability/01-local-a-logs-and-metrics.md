# Topic 17 / Local A: docker logs / kubectl logs and Metrics Reference

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Topics 1 and 8 complete.

Consolidate the logging and metrics commands you've used throughout this course into one reference pass.

## Step 1: Docker logs

```
docker run -d --name log-demo alpine sh -c 'while true; do echo tick; sleep 2; done'
docker logs log-demo
docker logs -f log-demo
```

Ctrl+C to stop following.


## Step 2: Docker stats

```
docker stats log-demo --no-stream
```


## Step 3: Kubernetes logs

```
kubectl create deployment web --image=nginx:1.27
kubectl logs deployment/web
```


## Step 4: Kubernetes resource usage (requires Metrics Server - install if not already present)

```
kubectl top pods 2>/dev/null || echo 'Metrics Server not installed - this is expected if you have not added it'
```


## Sanity checks

- You can retrieve logs and basic resource usage from both a plain container and a Kubernetes Pod.

Common places beginners get stuck: Only checking logs after something breaks (Get in the habit of glancing at logs during normal operation too, so you recognize what 'healthy' output looks like.)

## Cleanup

```
docker rm -f log-demo
kubectl delete deployment web
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

# Topic 9 / Local A: Quick Local Access with Port-Forward

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Topic 8 complete.

Learn the fastest way to temporarily reach a ClusterIP Service from your host, without setting up Ingress at all.

## Step 1: Create something to reach

```
kubectl create deployment web --image=nginx:1.27
kubectl expose deployment web --port=80
```


## Step 2: Port-forward to your host

```
kubectl port-forward service/web 8080:80 &
```


## Step 3: Test it

```
curl http://localhost:8080
```


## Step 4: Stop the port-forward

```
kill %1
```

Or press Ctrl+C if you ran it in the foreground instead.


## Sanity checks

- curl succeeds while the port-forward is running, and fails immediately after you kill it.

Common places beginners get stuck: Using port-forward as a real solution for external access (It's a debugging/dev convenience only - it stops the moment your terminal session ends, and isn't meant for real traffic.)

## Cleanup

```
kubectl delete deployment web
kubectl delete service web
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

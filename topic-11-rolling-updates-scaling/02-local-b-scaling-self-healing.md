# Topic 11 / Local B: Manual Scaling and Another Self-Healing Demo

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Local A complete.

Scale up and down deliberately, and reinforce self-healing from a different angle.

## Step 1: Deploy and scale up

```
kubectl create deployment web --image=nginx:1.27 --replicas=2
kubectl scale deployment web --replicas=5
kubectl get pods -w
```

Ctrl+C once all 5 are Running.


## Step 2: Scale back down

```
kubectl scale deployment web --replicas=1
kubectl get pods
```


## Step 3: Delete the last remaining Pod and watch it come back

```
kubectl delete pod $(kubectl get pods -l app=web -o jsonpath='{.items[0].metadata.name}')
kubectl get pods -w
```


## Sanity checks

- After scaling to 1 and deleting that single Pod, exactly 1 replacement Pod appears - proving the desired-state count (1) is what's enforced, not a fixed original Pod.

Common places beginners get stuck: Confusing kubectl scale with kubectl set image (scale changes replica COUNT; set image changes the container image - different operations, often used together but not interchangeable.)

## Cleanup

```
kubectl delete deployment web
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

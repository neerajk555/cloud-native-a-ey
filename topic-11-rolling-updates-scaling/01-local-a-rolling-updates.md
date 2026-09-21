# Topic 11 / Local A: Rolling Updates and Rollbacks

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Topic 8 complete.

Perform a zero-downtime update, then roll it back.

## Step 1: Deploy version 1

```
kubectl create deployment web --image=nginx:1.26 --replicas=4
```


## Step 2: Update to version 2

```
kubectl set image deployment/web nginx=nginx:1.27
kubectl rollout status deployment/web
```

Watch how it replaces a few Pods at a time, not all at once.


## Step 3: See rollout history

```
kubectl rollout history deployment/web
```


## Step 4: Roll back

```
kubectl rollout undo deployment/web
kubectl rollout status deployment/web
```


## Step 5: Confirm the image reverted

```
kubectl get deployment web -o jsonpath='{.spec.template.spec.containers[0].image}'
```


## Sanity checks

- The final image shown is nginx:1.26 again, after the rollback.

Common places beginners get stuck: Panicking when a rollout appears stuck (Use `kubectl rollout status` and `kubectl describe pod <new-pod>` to see WHY before assuming it's broken.)

## Cleanup

```
kubectl delete deployment web
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

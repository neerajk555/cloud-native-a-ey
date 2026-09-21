# Topic 8 / Local D: Namespaces for Organization

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Local C complete.

See how namespaces isolate identically-named resources - the exact mechanism your AWS namespace relies on.

## Step 1: Create two namespaces

```
kubectl create namespace team-a
kubectl create namespace team-b
```


## Step 2: Deploy the SAME-named app into both

```
kubectl create deployment web --image=nginx:1.27 -n team-a
kubectl create deployment web --image=nginx:1.27 -n team-b
```


## Step 3: Confirm they're independent

```
kubectl delete deployment web -n team-a
kubectl get deployments -n team-b
```

Deleting in team-a doesn't affect team-b.


## Sanity checks

- team-b's web deployment still exists after deleting team-a's.

Common places beginners get stuck: Forgetting to switch back your default namespace (Check `kubectl config view --minify | grep namespace` if `kubectl get` shows unexpectedly empty results.)

## Cleanup

```
kubectl delete namespace team-a team-b
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

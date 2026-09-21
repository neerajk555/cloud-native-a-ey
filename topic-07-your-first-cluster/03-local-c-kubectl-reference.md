# Topic 7 / Local C: kubectl Basics: A Full Reference Lab

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Local B complete.

Practice the kubectl commands you'll use constantly, all in one place.

## Step 1: Create something to inspect

```
kubectl run ref-demo --image=nginx:1.27
```


## Step 2: get / describe

```
kubectl get pods -o wide
kubectl describe pod ref-demo
```

The Events section at the bottom of describe is often the FIRST place to look when something's wrong.


## Step 3: logs / exec

```
kubectl logs ref-demo
kubectl exec -it ref-demo -- bash
```

Type `exit` to leave the shell.


## Step 4: delete

```
kubectl delete pod ref-demo
```


## Sanity checks

- You've successfully run get, describe, logs, exec, and delete against a real Pod.

Common places beginners get stuck: Only using get and never describe when debugging (get shows current state; describe shows the HISTORY of events that explain how it got there.)

## Cleanup

```
kubectl delete pod ref-demo 2>/dev/null || true
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

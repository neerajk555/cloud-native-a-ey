# Topic 8 / Local A: Deployments: What They Add Over Bare Pods

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Topic 7 complete.

Create a Deployment and directly observe self-healing by deleting a Pod it manages.

## Step 1: Create a Deployment

```
kubectl create deployment web --image=nginx:1.27 --replicas=3
```


## Step 2: See the chain of objects it created

```
kubectl get deployments
kubectl get replicasets
kubectl get pods
```

1 Deployment -> 1 ReplicaSet -> 3 Pods.


## Step 3: Delete one Pod directly

```
kubectl get pods
# copy one pod name, then:
kubectl delete pod <paste-pod-name-here>
kubectl get pods -w
```

A replacement appears within seconds - THIS is self-healing.


## Sanity checks

- You end up with 3 Pods again, one of which has a different name than before you deleted it.

Common places beginners get stuck: Creating bare Pods for long-running apps instead of Deployments (Almost always use a Deployment specifically to get self-healing.)

## Cleanup

```
kubectl delete deployment web
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

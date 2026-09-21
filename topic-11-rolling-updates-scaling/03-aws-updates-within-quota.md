# Topic 11 / AWS: Rolling Updates and Scaling Within Your Namespace Quota

The same exercise as the local version, now done for real on AWS using your assigned IAM user credentials -
about 15 minutes. Estimated cost: ~$0.00-0.02 (a handful of small Pods running briefly on already-provisioned shared nodes), and only accurate if you complete the cleanup step below
right after finishing - don't leave this running "to come back to later." Prerequisites: Topic 11 Local A-B complete, Topic 8's AWS exercise complete.

Repeat rolling updates and scaling on your EKS namespace, this time discovering what happens when you hit your namespace's real resource quota - something your local kind cluster never enforced. This is the same concept you just practiced locally - the goal here is seeing how it plays out
under real AWS constraints (IAM permissions, tagging requirements, shared-account limits), not relearning the
concept itself.

## Step 1: Deploy and try to scale higher than you did locally

```
kubectl create deployment web --image=nginx:1.27 --replicas=2
kubectl scale deployment web --replicas=10
```


## Step 2: Check what actually happened

```
kubectl get pods
kubectl describe resourcequota
```

If some Pods are stuck Pending, check `kubectl describe pod <pending-pod-name>` for a message about exceeding your namespace's quota - this is expected and intentional, not a bug.


## Step 3: Scale back to something your quota allows

```
kubectl scale deployment web --replicas=2
```


## Step 4: Perform a rolling update, same as locally

```
kubectl set image deployment/web nginx=nginx:1.28
kubectl rollout status deployment/web
```


## Sanity checks

- You can explain, in your own words, why scaling to 10 didn't fully succeed here even though it would have on your local kind cluster.

Common places beginners get stuck: Assuming a quota-blocked scale-up is an error to fix by contacting the instructor (It's a deliberate, shared-account cost/capacity control - simply scale to a value within your quota instead.) Leaving replicas at a high count after this exercise (Scale back down as part of cleanup - other participants share this cluster's total node capacity.)

## Cleanup (do this now, not later)

```
kubectl delete deployment web
```

Then complete the mandatory checklist, `04-cleanup-checklist.md`, before moving to the next topic.

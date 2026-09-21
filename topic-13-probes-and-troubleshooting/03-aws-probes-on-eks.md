# Topic 13 / AWS: The Same Probes on Your EKS Namespace

The same exercise as the local version, now done for real on AWS using your assigned IAM user credentials -
about 15 minutes. Estimated cost: ~$0.00-0.01 (a single small Pod running briefly), and only accurate if you complete the cleanup step below
right after finishing - don't leave this running "to come back to later." Prerequisites: Topic 13 Local A-B complete, Topic 8's AWS exercise complete.

Confirm readiness/liveness probes behave identically on real EKS infrastructure - the YAML doesn't change at all, only the cluster it's applied to. This is the same concept you just practiced locally - the goal here is seeing how it plays out
under real AWS constraints (IAM permissions, tagging requirements, shared-account limits), not relearning the
concept itself.

## Step 1: Apply the same probes.yaml from Local A, in your namespace

```
kubectl apply -f ~/course/k8s-demo/probes.yaml
kubectl expose deployment web --port=80
```


## Step 2: Confirm readiness works the same way

```
kubectl get pods
kubectl get endpoints web
```


## Sanity checks

- Identical behavior to Local A - same YAML, same result, different cluster.

Common places beginners get stuck: Expecting AWS-specific probe syntax (There isn't any - Kubernetes probes are identical regardless of which cluster they run on; this is exactly the portability Kubernetes is designed for.)

## Cleanup (do this now, not later)

```
kubectl delete -f ~/course/k8s-demo/probes.yaml
kubectl delete service web
```

Then complete the mandatory checklist, `04-cleanup-checklist.md`, before moving to the next topic.

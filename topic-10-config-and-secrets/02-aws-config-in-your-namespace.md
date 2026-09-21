# Topic 10 / AWS: Same Pattern on EKS, Compared to Secrets Manager

The same exercise as the local version, now done for real on AWS using your assigned IAM user credentials -
about 15 minutes. Estimated cost: $0.00-0.01 (a few minutes of a small Deployment; Secrets Manager itself is not created, just discussed/compared), and only accurate if you complete the cleanup step below
right after finishing - don't leave this running "to come back to later." Prerequisites: Topic 10 Local A complete, Topic 8's AWS exercise complete.

Repeat the ConfigMap/Secret pattern in your real namespace, then look at how AWS Secrets Manager differs for values that need REAL protection. This is the same concept you just practiced locally - the goal here is seeing how it plays out
under real AWS constraints (IAM permissions, tagging requirements, shared-account limits), not relearning the
concept itself.

## Step 1: Repeat the same pattern in your namespace

```
kubectl create configmap app-config --from-literal=GREETING=hello
kubectl create secret generic app-secret --from-literal=API_KEY=demo123
kubectl create deployment web --image=nginx:1.27
kubectl set env deployment/web --from=configmap/app-config
kubectl set env deployment/web --from=secret/app-secret
```


## Step 2: Compare: look at what Secrets Manager would give you instead (read-only, no creation needed)

```
aws secretsmanager list-secrets --region us-east-1
```

This will likely be empty for your account - that's fine, the point is understanding the OPTION exists, not using it in this exercise.


## Step 3: Read: why you'd reach for Secrets Manager instead of a plain K8s Secret for something real


AWS Secrets Manager encrypts values at rest with KMS by default, supports automatic rotation, and provides audit logging of every read - none of which a plain Kubernetes Secret gives you out of the box.


## Sanity checks

- Your Deployment's Pod shows both env vars correctly, and you can explain when you'd reach for Secrets Manager instead of a plain K8s Secret.

Common places beginners get stuck: Assuming this exercise requires creating an actual Secrets Manager secret (It doesn't - the comparison is conceptual; creating one isn't necessary to understand the trade-off, and avoids extra cleanup for zero learning benefit.)

## Cleanup (do this now, not later)

```
kubectl delete deployment web
kubectl delete configmap app-config
kubectl delete secret app-secret
```

Then complete the mandatory checklist, `03-cleanup-checklist.md`, before moving to the next topic.

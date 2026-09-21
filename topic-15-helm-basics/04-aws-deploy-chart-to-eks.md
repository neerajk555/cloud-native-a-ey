# Topic 15 / AWS: Deploy Your Chart to Your EKS Namespace

The same exercise as the local version, now done for real on AWS using your assigned IAM user credentials -
about 15 minutes. Estimated cost: ~$0.00-0.02 (a few small Pods running briefly on already-provisioned shared nodes), and only accurate if you complete the cleanup step below
right after finishing - don't leave this running "to come back to later." Prerequisites: Topic 15 Local A-C complete, Topic 8's AWS exercise complete.

Install the exact chart you built and tested locally onto your real EKS namespace - proving the core promise of Helm: one chart, multiple environments. This is the same concept you just practiced locally - the goal here is seeing how it plays out
under real AWS constraints (IAM permissions, tagging requirements, shared-account limits), not relearning the
concept itself.

## Step 1: Confirm you're pointed at your EKS namespace

```
kubectl config current-context
kubectl config view --minify | grep namespace
```


## Step 2: Install your chart from Local C, unchanged

```
cd ~/course/mychart
helm install eks-test ./mychart-0.2.0.tgz
```


## Step 3: Verify it deployed correctly

```
kubectl get pods -l app.kubernetes.io/instance=eks-test
kubectl exec $(kubectl get pods -l app.kubernetes.io/instance=eks-test -o jsonpath='{.items[0].metadata.name}') -- env | grep GREETING
```


## Sanity checks

- The exact same chart, with zero modifications, deploys correctly to real EKS infrastructure.

Common places beginners get stuck: Modifying the chart 'for AWS' before deploying (That defeats the point of this exercise - the whole value of Helm is that the SAME chart works across environments without modification.)

## Cleanup (do this now, not later)

```
helm uninstall eks-test
```

Then complete the mandatory checklist, `05-cleanup-checklist.md`, before moving to the next topic.

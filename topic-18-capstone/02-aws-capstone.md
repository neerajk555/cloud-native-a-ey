# Topic 18 / AWS: Deploy the Capstone to Your EKS Namespace

The same exercise as the local version, now done for real on AWS using your assigned IAM user credentials -
about 15 minutes. Estimated cost: ~$0.01-0.03 (a handful of small Pods and one completed Job, running briefly on already-provisioned shared nodes), and only accurate if you complete the cleanup step below
right after finishing - don't leave this running "to come back to later." Prerequisites: Topic 18 Local capstone complete and fully passing its self-check, Topic 8's AWS exercise complete (namespace verified).

Deploy the exact chart you built and fully tested locally onto your real EKS namespace - the final proof that everything in this course transfers directly to real infrastructure. This is the same concept you just practiced locally - the goal here is seeing how it plays out
under real AWS constraints (IAM permissions, tagging requirements, shared-account limits), not relearning the
concept itself.

## Step 1: Confirm you're pointed at your EKS namespace

```
kubectl config current-context
kubectl config view --minify | grep namespace
```


## Step 2: Install your unmodified chart

```
cd ~/course/mychart
helm install capstone .
```


## Step 3: Run through the same self-check from the local exercise, against EKS now


- [ ] Pods and Job status correct
- [ ] worker's logs show the right ConfigMap value
- [ ] Scale api up via helm upgrade, then roll back
- [ ] Break and fix api's readiness probe, confirming the endpoints behavior matches what you saw locally


## Sanity checks

- Every item from your local self-check also passes here, with zero chart modifications - proof the same artifact works across environments.

Common places beginners get stuck: Modifying the chart 'for AWS' (The whole point is that it shouldn't need to change - if it does, that's worth understanding why before considering the capstone done.) Leaving this running after finishing the course (This is the last topic - leaving Job/Deployment/Service objects in your namespace after the course ends is exactly the kind of drift the cleanup checklists throughout this course were meant to prevent.)

## Cleanup (do this now, not later)

```
helm uninstall capstone
```

Then complete the mandatory checklist, `03-cleanup-checklist.md`, before moving to the next topic.

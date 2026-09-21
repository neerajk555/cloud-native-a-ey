# Topic 14 / AWS: The Same Job and CronJob YAML on Your EKS Namespace

The same exercise as the local version, now done for real on AWS using your assigned IAM user credentials -
about 15 minutes. Estimated cost: $0.00-0.01 (Jobs complete in seconds on already-provisioned shared nodes), and only accurate if you complete the cleanup step below
right after finishing - don't leave this running "to come back to later." Prerequisites: Topic 14 Local A-C complete, Topic 8's AWS exercise complete.

Confirm Jobs and CronJobs behave identically on real EKS infrastructure - reusing exactly what you already know from Local A, in your own namespace. This is the same concept you just practiced locally - the goal here is seeing how it plays out
under real AWS constraints (IAM permissions, tagging requirements, shared-account limits), not relearning the
concept itself.

## Step 1: Create the same Job in your namespace

```
kubectl create job hello-job --image=busybox -- echo 'batch task complete on EKS'
kubectl wait --for=condition=complete job/hello-job --timeout=60s
kubectl logs job/hello-job
```


## Sanity checks

- The Job completes and its logs show your custom message, exactly as it did locally.

Common places beginners get stuck: Leaving a CronJob running in your namespace after this exercise (Unlike a one-shot Job, a CronJob keeps creating new Jobs forever until deleted - always delete it, not just its most recent Job.)

## Cleanup (do this now, not later)

```
kubectl delete job hello-job
```

Then complete the mandatory checklist, `05-cleanup-checklist.md`, before moving to the next topic.

# Topic 7 / AWS: Connect to the Shared Cluster and Verify Your Namespace

The same exercise as the local version, now done for real on AWS using your assigned IAM user credentials -
about 15 minutes. Estimated cost: $0.00 for you - the shared cluster's cost is covered by the instructor's infrastructure budget, not per-participant, and only accurate if you complete the cleanup step below
right after finishing - don't leave this running "to come back to later." Prerequisites: Topic 7 Local A-C complete, Instructor has run namespace setup for this cohort and confirmed your namespace exists.

Point kubectl at the real shared EKS cluster (already created by your instructor) and confirm your own namespace exists, exactly the way you'd expect after Topic 6's exploration. This is the same concept you just practiced locally - the goal here is seeing how it plays out
under real AWS constraints (IAM permissions, tagging requirements, shared-account limits), not relearning the
concept itself.

## Step 1: Update kubeconfig to point at the shared cluster

```
aws eks update-kubeconfig --name course-shared-cluster --region us-east-1
```


## Step 2: Set your namespace as your default context

```
kubectl config set-context --current --namespace=ns-$PARTICIPANT
```


## Step 3: Verify you can see your own namespace's resources

```
kubectl get all
```


## Step 4: Verify you CANNOT see someone else's namespace (this is intentional, not a bug)

```
kubectl get pods -n ns-someone-else-fake-name
```

Expect a Forbidden or NotFound error here - your RBAC access is scoped to your own namespace only.


## Sanity checks

`kubectl get all` in your own namespace succeeds (even if empty); attempting another namespace is denied.

Common places beginners get stuck: Trying to create a cluster yourself (You don't have `eks:CreateCluster` for good reason (cost, quota) - always connect to the instructor-provisioned shared cluster instead.) Forgetting to switch your namespace context and wondering why kubectl shows nothing (Re-run the set-context command from Step 2 - it's easy to lose this after switching between local kind and the shared cluster.)

## Cleanup (do this now, not later)

```
# Nothing was created here - connecting and verifying access has no cost or cleanup.
```

Then complete the mandatory checklist, `05-cleanup-checklist.md`, before moving to the next topic.

# Topic 6 / AWS: Explore the Shared EKS Cluster's Control Plane (Read-Only)

The same exercise as the local version, now done for real on AWS using your assigned IAM user credentials -
about 15 minutes. Estimated cost: $0.00 - this is entirely read-only, nothing is created, and only accurate if you complete the cleanup step below
right after finishing - don't leave this running "to come back to later." Prerequisites: Topic 6 Local A-C complete, Instructor has confirmed the shared EKS cluster is up for this cohort.

Look at a REAL managed Kubernetes control plane and compare it to the concepts you just read about, without creating anything yet. Your instructor has already provisioned this shared cluster for the cohort. This is the same concept you just practiced locally - the goal here is seeing how it plays out
under real AWS constraints (IAM permissions, tagging requirements, shared-account limits), not relearning the
concept itself.

## Step 1: Point kubectl at the shared cluster

```
aws eks update-kubeconfig --name course-shared-cluster --region us-east-1
```


## Step 2: See the cluster's control plane details

```
aws eks describe-cluster --name course-shared-cluster --region us-east-1 --query 'cluster.{Status:status,Endpoint:endpoint,Version:version}'
```


## Step 3: See the worker nodes (this IS the 'nodes' from Local B, for real)

```
kubectl get nodes
```


## Step 4: See that a control plane component you never manage is running this for you

```
kubectl cluster-info
```

Notice you never provisioned or patched this control plane yourself — that's what 'managed' means in Amazon EKS.


## Sanity checks

`kubectl get nodes` returns at least one node in Ready status — real infrastructure your instructor set up, not something on your own VM.

Common places beginners get stuck: Trying to create or delete anything here (This exercise is intentionally read-only — you don't have cluster-admin permissions, and creating real workloads starts in Topic 7.) Confusing this shared cluster with your own kind cluster from Topic 7 (They're different clusters entirely — this one is real AWS infrastructure shared by the whole cohort; kind is free and lives only on your VM.)

## Cleanup (do this now, not later)

```
# Nothing was created - nothing to delete. This exercise has zero cleanup.
```

Then complete the mandatory checklist, `05-cleanup-checklist.md`, before moving to the next topic.

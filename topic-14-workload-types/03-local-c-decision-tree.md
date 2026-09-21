# Topic 14 / Local C: Choosing the Right Workload Type

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Local A and B complete.

Apply everything from this topic to 8 real-world scenarios, no new commands needed.

## Step 1: The decision tree


1. Runs on EVERY node? -> DaemonSet (not covered hands-on in this course, but good to recognize)
2. Runs once/occasionally to completion? -> Job (one-off) or CronJob (scheduled)
3. Needs stable identity / ordered startup? -> StatefulSet
4. Otherwise (stateless, continuous, interchangeable) -> Deployment


## Step 2: Classify these 6 scenarios yourself first


A) A stateless REST API with 5 replicas
B) A nightly database backup script
C) A 3-node database cluster with distinct broker IDs and storage
D) A one-time database schema migration
E) A React frontend via nginx, 3 replicas
F) A weekly report-generation task


## Sanity checks

- Correct answers: A) Deployment, B) CronJob, C) StatefulSet, D) Job, E) Deployment, F) CronJob.

Common places beginners get stuck: Defaulting to Deployment for everything (It's the most common choice, but check the decision tree, especially for anything stateful or scheduled.)

## Cleanup

```
# Exercise-only, nothing to clean up.
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

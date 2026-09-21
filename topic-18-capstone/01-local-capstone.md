# Topic 18 / Local: Build and Test the Capstone App on kind

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: All of Topics 1-17 complete.

Build a small multi-service app combining Docker, Kubernetes, and Helm skills from every prior topic, and get it fully working on your free local cluster before touching AWS at all.

## Step 1: The spec


Build an app with THREE pieces:

1. **`api`** - a small HTTP service that returns JSON, reads at least one setting from a ConfigMap-injected environment variable (Topic 10), and has a working readiness probe (Topic 13)
2. **`worker`** - a Job (Topic 14, not a long-running service) that runs once and prints a message referencing the same ConfigMap value
3. **`web`** - nginx serving a static page

Package all of this as a **single Helm chart** (Topic 15) with a values.yaml exposing image tags, the ConfigMap's values, and api's replica count.


## Step 2: Build order


1. Get each piece working with plain `docker run` first (Topics 1-3)
2. Write raw Kubernetes YAML and get it working with `kubectl apply` on kind (Topics 7-14)
3. Convert the working YAML into a templated Helm chart (Topic 15)


## Step 3: Self-check before moving to AWS


- [ ] `helm install capstone ./mychart` succeeds
- [ ] api and web Pods show Running with READY 1/1; worker's Job shows Completed
- [ ] `kubectl logs job/worker-<...>` shows it correctly read the ConfigMap value
- [ ] `helm upgrade capstone ./mychart --set api.replicaCount=3` scales only api
- [ ] `helm rollback capstone 1` correctly reverts that change
- [ ] Deliberately breaking api's readiness probe removes it from traffic without a restart (prove with `kubectl get endpoints`)


## Sanity checks

- All checklist items above pass on your local kind cluster.

Common places beginners get stuck: Jumping straight to Helm without raw YAML working first (Debugging templating AND app issues simultaneously is much harder - always get plain kubectl apply working first.)

## Cleanup

```
helm uninstall capstone 2>/dev/null || true
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

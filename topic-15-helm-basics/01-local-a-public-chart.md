# Topic 15 / Local A: Installing a Public Chart

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Topic 8 complete, Helm installed.

Use a chart someone else wrote before authoring your own - also the most common real-world Helm use case.

## Step 1: Add a repo and see available values

```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm show values bitnami/nginx | head -30
```


## Step 2: Install it

```
helm install my-nginx bitnami/nginx
```


## Step 3: Customize and upgrade

```
helm upgrade my-nginx bitnami/nginx --set replicaCount=3
kubectl get pods -l app.kubernetes.io/instance=my-nginx
```


## Step 4: Uninstall cleanly

```
helm uninstall my-nginx
```


## Sanity checks

- 3 Pods appear after the upgrade, and `helm uninstall` removes everything without a trace in `kubectl get all`.

Common places beginners get stuck: Using kubectl delete on a Helm-installed app (Always use `helm uninstall` for anything installed via Helm, to avoid orphaned resources.)

## Cleanup

```
helm uninstall my-nginx 2>/dev/null || true
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

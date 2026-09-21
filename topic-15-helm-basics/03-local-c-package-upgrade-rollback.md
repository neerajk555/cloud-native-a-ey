# Topic 15 / Local C: Packaging, Versioning, Upgrading, Rollback

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Local B complete.

Package your chart, install it, upgrade it, and roll back a change.

## Step 1: Package and install

```
cd ~/course/mychart
helm package .
helm install final-test ./mychart-0.1.0.tgz
```


## Step 2: Bump version, change a value, repackage, upgrade

```
sed -i 's/version: 0.1.0/version: 0.2.0/' Chart.yaml
sed -i 's/replicaCount: 1/replicaCount: 3/' values.yaml
helm package .
helm upgrade final-test ./mychart-0.2.0.tgz
```


## Step 3: See history and roll back

```
helm history final-test
helm rollback final-test 1
kubectl get pods -l app.kubernetes.io/instance=final-test
```


## Sanity checks

- After rollback, replica count is back to 1.

Common places beginners get stuck: Forgetting to bump Chart.yaml's version before repackaging (Helm won't stop you from reusing a version number, but it defeats the purpose - always bump on real changes.)

## Cleanup

```
helm uninstall final-test
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

# Topic 10 / Local A: ConfigMaps and Secrets

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Topic 8 complete.

Inject both into a Deployment as environment variables, and prove Secrets aren't truly encrypted by default.

## Step 1: Create a ConfigMap and a Secret

```
kubectl create configmap app-config --from-literal=GREETING=hello
kubectl create secret generic app-secret --from-literal=API_KEY=demo123
```


## Step 2: Create a Deployment and inject both

```
kubectl create deployment web --image=nginx:1.27
kubectl set env deployment/web --from=configmap/app-config
kubectl set env deployment/web --from=secret/app-secret
```


## Step 3: Verify inside the Pod

```
kubectl get pods
kubectl exec <paste-pod-name> -- env | grep -E 'GREETING|API_KEY'
```


## Step 4: Prove base64 isn't encryption

```
kubectl get secret app-secret -o jsonpath='{.data.API_KEY}' | base64 -d
```

This decodes instantly with a standard command - anyone with read access to the Secret object can do this.


## Sanity checks

- The decoded output shows `demo123` in plain text.

Common places beginners get stuck: Treating Secrets as automatically secure (Base64 is an ENCODING, not encryption - real protection needs cluster-level encryption-at-rest AND tight RBAC restricting who can read Secret objects at all.)

## Cleanup

```
kubectl delete deployment web
kubectl delete configmap app-config
kubectl delete secret app-secret
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

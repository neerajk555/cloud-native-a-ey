# Topic 13 / Local A: Readiness and Liveness Probes

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Topic 8 complete, Topic 4 complete.

Configure both probe types and see the real difference between them.

## Step 1: Deploy with both probes

```
cat > ~/course/k8s-demo/probes.yaml <<'EOF'
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
spec:
  replicas: 1
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
        - name: nginx
          image: nginx:1.27
          readinessProbe:
            httpGet:
              path: /
              port: 80
            periodSeconds: 5
          livenessProbe:
            httpGet:
              path: /
              port: 80
            periodSeconds: 10
EOF
kubectl apply -f ~/course/k8s-demo/probes.yaml
kubectl expose deployment web --port=80
```


**Using VS Code instead:** rather than the heredoc above, you can do this directly in the editor.

In VS Code's Explorer, create a new file named `~/course/k8s-demo/probes.yaml`, paste this, and save (`Ctrl+S`):

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
spec:
  replicas: 1
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
        - name: nginx
          image: nginx:1.27
          readinessProbe:
            httpGet:
              path: /
              port: 80
            periodSeconds: 5
          livenessProbe:
            httpGet:
              path: /
              port: 80
            periodSeconds: 10
```


## Step 2: Confirm it's ready

```
kubectl get pods
```

READY shows 1/1 once the readiness probe passes.


## Step 3: Break ONLY readiness (change the path to a 404) and observe

```
kubectl get endpoints web
# Edit probes.yaml's readinessProbe path to /does-not-exist, then:
kubectl apply -f ~/course/k8s-demo/probes.yaml
kubectl get endpoints web
```

Endpoints becomes empty even though `kubectl get pods` still shows the Pod as Running - it's removed from traffic, not restarted.


## Sanity checks

- The Pod stays Running throughout, but disappears from `kubectl get endpoints web` once readiness starts failing.

Common places beginners get stuck: Only configuring a liveness probe (A liveness-only setup means a temporarily overloaded (but not crashed) Pod keeps receiving traffic it can't handle - configure both for real apps.)

## Cleanup

```
kubectl delete -f ~/course/k8s-demo/probes.yaml
kubectl delete service web
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

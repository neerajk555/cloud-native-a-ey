# Topic 14 / Local B: StatefulSets vs. Deployments

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Local A complete.

See the predictable, ordered naming a StatefulSet gives you, and understand when that actually matters.

## Step 1: Compare Deployment naming (random)

```
kubectl create deployment web --image=nginx:1.27 --replicas=2
kubectl get pods -l app=web
```

Names look random, e.g. web-7d9f8c9b76-x7k2p.


## Step 2: Create a headless Service + StatefulSet

```
cat > ~/course/k8s-demo/sts.yaml <<'EOF'
apiVersion: v1
kind: Service
metadata:
  name: stateful-svc
spec:
  clusterIP: None
  selector:
    app: stateful-demo
  ports:
    - port: 80
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: stateful-demo
spec:
  serviceName: stateful-svc
  replicas: 3
  selector:
    matchLabels:
      app: stateful-demo
  template:
    metadata:
      labels:
        app: stateful-demo
    spec:
      containers:
        - name: nginx
          image: nginx:1.27
EOF
kubectl apply -f ~/course/k8s-demo/sts.yaml
```


**Using VS Code instead:** rather than the heredoc above, you can do this directly in the editor.

In VS Code's Explorer, create a new file named `~/course/k8s-demo/sts.yaml`, paste this, and save (`Ctrl+S`):

```
apiVersion: v1
kind: Service
metadata:
  name: stateful-svc
spec:
  clusterIP: None
  selector:
    app: stateful-demo
  ports:
    - port: 80
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: stateful-demo
spec:
  serviceName: stateful-svc
  replicas: 3
  selector:
    matchLabels:
      app: stateful-demo
  template:
    metadata:
      labels:
        app: stateful-demo
    spec:
      containers:
        - name: nginx
          image: nginx:1.27
```


## Step 3: See predictable, ordered naming

```
kubectl get pods -l app=stateful-demo
```

Names are stateful-demo-0, -1, -2, always in that order.


## Step 4: Delete one specifically and confirm its replacement keeps the SAME name

```
kubectl delete pod stateful-demo-1
kubectl get pods -l app=stateful-demo
```


## Sanity checks

- stateful-demo-1's replacement is still named stateful-demo-1, unlike the Deployment's randomly-named replacements.

Common places beginners get stuck: Using a StatefulSet for a stateless web app (Unnecessary complexity - only use it when you specifically need stable identity or ordered startup.)

## Cleanup

```
kubectl delete -f ~/course/k8s-demo/sts.yaml
kubectl delete deployment web
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

# Topic 8 / Local C: Labels and Selectors

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Local B complete.

See exactly how a Service's selector determines which Pods it routes to, by breaking and fixing it yourself.

## Step 1: Create a Deployment

```
kubectl create deployment web --image=nginx:1.27 --replicas=2
kubectl get pods --show-labels
```


## Step 2: Create a Service with a selector that DOESN'T match

```
cat > ~/course/k8s-demo/bad-service.yaml <<'EOF'
apiVersion: v1
kind: Service
metadata:
  name: broken-web
spec:
  selector:
    app: wrong-label
  ports:
    - port: 80
EOF
kubectl apply -f ~/course/k8s-demo/bad-service.yaml
```


**Using VS Code instead:** rather than the heredoc above, you can do this directly in the editor.

In VS Code's Explorer, create a new file named `~/course/k8s-demo/bad-service.yaml`, paste this, and save (`Ctrl+S`):

```
apiVersion: v1
kind: Service
metadata:
  name: broken-web
spec:
  selector:
    app: wrong-label
  ports:
    - port: 80
```


## Step 3: See the broken result

```
kubectl get endpoints broken-web
```

Empty - the selector matches nothing.


## Step 4: Fix it

```
kubectl delete -f ~/course/k8s-demo/bad-service.yaml
kubectl expose deployment web --port=80 --name=fixed-web
kubectl get endpoints fixed-web
```

Now shows real Pod IPs.


## Sanity checks

- fixed-web's endpoints list actual Pod IPs; broken-web's did not.

Common places beginners get stuck: Assuming a Service is broken due to networking when it's a label mismatch (Always check `kubectl get endpoints <service>` first - an empty list points at a selector problem.)

## Cleanup

```
kubectl delete deployment web
kubectl delete service fixed-web broken-web 2>/dev/null || true
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

# Topic 7 / Local B: Your First Pod (Raw YAML)

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Local A complete.

Write raw Pod YAML once, deliberately, to understand the structure - even though you'll almost always use Deployments (Topic 8) instead in practice.

## Step 1: Write the YAML

```
mkdir -p ~/course/k8s-demo && cd ~/course/k8s-demo
cat > pod.yaml <<'EOF'
apiVersion: v1
kind: Pod
metadata:
  name: my-first-pod
spec:
  containers:
    - name: nginx
      image: nginx:1.27
EOF
```


**Using VS Code instead:** rather than the heredoc above, you can do this directly in the editor.

In VS Code's Explorer, create a new file named `pod.yaml`, paste this, and save (`Ctrl+S`):

```
apiVersion: v1
kind: Pod
metadata:
  name: my-first-pod
spec:
  containers:
    - name: nginx
      image: nginx:1.27
```


## Step 2: Apply it

```
kubectl apply -f pod.yaml
```


## Step 3: Watch it come up

```
kubectl get pods -w
```

Ctrl+C once STATUS shows Running.


## Step 4: Inspect and see logs

```
kubectl describe pod my-first-pod
kubectl logs my-first-pod
```


## Sanity checks

`kubectl get pods` shows my-first-pod as Running 1/1.

Common places beginners get stuck: Expecting a deleted bare Pod to come back on its own (It won't - only Pods managed by a Deployment get recreated automatically (Topic 8).) Using tabs in YAML (YAML requires spaces, not tabs, for indentation.)

## Cleanup

```
kubectl delete -f pod.yaml
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

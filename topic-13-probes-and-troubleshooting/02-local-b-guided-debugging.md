# Topic 13 / Local B: Troubleshooting a Broken Pod (Guided Exercise)

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Local A complete.

Diagnose three intentionally broken Pods, learning to recognize their status patterns instantly.

## Step 1: Failure 1: image typo

```
kubectl run broken1 --image=ngnix:1.27
kubectl get pods
kubectl describe pod broken1
```

Look for ImagePullBackOff / ErrImagePull.


## Step 2: Failure 2: resource request too high

```
cat > ~/course/k8s-demo/broken2.yaml <<'EOF'
apiVersion: v1
kind: Pod
metadata:
  name: broken2
spec:
  containers:
    - name: hog
      image: nginx:1.27
      resources:
        requests:
          memory: "500Gi"
EOF
kubectl apply -f ~/course/k8s-demo/broken2.yaml
kubectl describe pod broken2
```


**Using VS Code instead:** rather than the heredoc above, you can do this directly in the editor.

In VS Code's Explorer, create a new file named `~/course/k8s-demo/broken2.yaml`, paste this, and save (`Ctrl+S`):

```
apiVersion: v1
kind: Pod
metadata:
  name: broken2
spec:
  containers:
    - name: hog
      image: nginx:1.27
      resources:
        requests:
          memory: "500Gi"
```

Look for Pending status and an Insufficient memory event.


## Step 3: Failure 3: crashing app

```
kubectl run broken3 --image=alpine -- sh -c 'exit 1'
kubectl get pods
kubectl logs broken3
```

Look for CrashLoopBackOff.


## Sanity checks

- You can correctly name all three failure statuses (ImagePullBackOff, Pending, CrashLoopBackOff) without looking them up.

Common places beginners get stuck: Jumping straight to logs for a Pending or ImagePullBackOff Pod (Those fail BEFORE the container ever starts - there are no logs yet; use describe/Events instead.)

## Cleanup

```
kubectl delete pod broken1 broken3 2>/dev/null || true
kubectl delete -f ~/course/k8s-demo/broken2.yaml 2>/dev/null || true
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

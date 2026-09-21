# Topic 12 / Local A: Persistent Volumes and Claims, From First Principles

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Topic 8 complete, Topic 3's volume concepts.

Create a PVC, use it in a Pod, and prove data survives Pod deletion.

## Step 1: Create a PVC (kind auto-provisions the storage)

```
cat > ~/course/k8s-demo/pvc.yaml <<'EOF'
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
EOF
kubectl apply -f ~/course/k8s-demo/pvc.yaml
kubectl get pvc
```


**Using VS Code instead:** rather than the heredoc above, you can do this directly in the editor.

In VS Code's Explorer, create a new file named `~/course/k8s-demo/pvc.yaml`, paste this, and save (`Ctrl+S`):

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```


## Step 2: Mount it in a Pod and write data

```
cat > ~/course/k8s-demo/pod-with-pvc.yaml <<'EOF'
apiVersion: v1
kind: Pod
metadata:
  name: pvc-demo
spec:
  containers:
    - name: app
      image: busybox
      command: ["sleep", "3600"]
      volumeMounts:
        - name: data
          mountPath: /data
  volumes:
    - name: data
      persistentVolumeClaim:
        claimName: my-pvc
EOF
kubectl apply -f ~/course/k8s-demo/pod-with-pvc.yaml
kubectl exec pvc-demo -- sh -c 'echo persisted-data > /data/file.txt'
```


**Using VS Code instead:** rather than the heredoc above, you can do this directly in the editor.

In VS Code's Explorer, create a new file named `~/course/k8s-demo/pod-with-pvc.yaml`, paste this, and save (`Ctrl+S`):

```
apiVersion: v1
kind: Pod
metadata:
  name: pvc-demo
spec:
  containers:
    - name: app
      image: busybox
      command: ["sleep", "3600"]
      volumeMounts:
        - name: data
          mountPath: /data
  volumes:
    - name: data
      persistentVolumeClaim:
        claimName: my-pvc
```


## Step 3: Delete and recreate the Pod, same PVC

```
kubectl delete pod pvc-demo
kubectl apply -f ~/course/k8s-demo/pod-with-pvc.yaml
kubectl exec pvc-demo -- cat /data/file.txt
```


## Sanity checks

- The file's content prints correctly even though the Pod was fully deleted and recreated in between.

Common places beginners get stuck: Assuming deleting the Pod deletes the data (That's the whole point of PVCs - data outlives the Pod, tied to the PVC/PV instead.)

## Cleanup

```
kubectl delete -f ~/course/k8s-demo/pod-with-pvc.yaml -f ~/course/k8s-demo/pvc.yaml
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

# Topic 8 / Local B: Services: ClusterIP Explained

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Local A complete.

Create a Service and prove its address stays stable even as the Pods behind it are replaced.

## Step 1: Create a Deployment and expose it

```
kubectl create deployment web --image=nginx:1.27 --replicas=3
kubectl expose deployment web --port=80
```


## Step 2: See the stable ClusterIP

```
kubectl get service web
```


## Step 3: Hit it from another Pod

```
kubectl run tester --image=alpine --rm -it -- sh -c 'wget -qO- http://web'
```


## Step 4: Delete a backing Pod and confirm the Service address doesn't change

```
kubectl get pods
kubectl delete pod <paste-one-pod-name>
kubectl get service web
```

Same ClusterIP as before.


## Sanity checks

- The Service's CLUSTER-IP value is identical before and after deleting a Pod.

Common places beginners get stuck: Expecting to curl a ClusterIP Service from your host machine directly (ClusterIP is cluster-internal only by default - use a temporary Pod to test, as shown.)

## Cleanup

```
kubectl delete deployment web
kubectl delete service web
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

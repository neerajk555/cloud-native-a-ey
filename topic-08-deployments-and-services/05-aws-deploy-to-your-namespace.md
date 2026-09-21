# Topic 8 / AWS: Deploy the Same App to Your EKS Namespace

The same exercise as the local version, now done for real on AWS using your assigned IAM user credentials -
about 15 minutes. Estimated cost: ~$0.00-0.02 (a few nginx Pods running briefly on already-provisioned shared nodes), and only accurate if you complete the cleanup step below
right after finishing - don't leave this running "to come back to later." Prerequisites: Topic 8 Local A-D complete, Topic 7's AWS exercise complete (namespace verified).

Apply the exact same Deployment/Service YAML from Local A-C, but inside your own namespace on the real shared EKS cluster. This is the same concept you just practiced locally - the goal here is seeing how it plays out
under real AWS constraints (IAM permissions, tagging requirements, shared-account limits), not relearning the
concept itself.

## Step 1: Confirm you're pointed at the shared cluster and your namespace

```
kubectl config current-context
kubectl config view --minify | grep namespace
```


## Step 2: Create the Deployment and Service - identical commands to Local B

```
kubectl create deployment web --image=nginx:1.27 --replicas=2
kubectl expose deployment web --port=80
```


## Step 3: Verify it's running in YOUR namespace only

```
kubectl get pods,svc
```


## Step 4: Prove self-healing works here too

```
kubectl delete pod $(kubectl get pods -l app=web -o jsonpath='{.items[0].metadata.name}')
kubectl get pods -w
```


## Sanity checks

- You see 2 Pods and 1 Service, scoped to your namespace, and a deleted Pod gets replaced automatically - identical behavior to your local kind cluster.

Common places beginners get stuck: Requesting more than 2 replicas without checking your namespace's ResourceQuota (Your namespace has a quota set by the instructor - check `kubectl describe resourcequota` if a Pod stays Pending.) Forgetting this is REAL shared infrastructure (Other participants' namespaces are on the same cluster - always clean up promptly so your workloads don't count against the shared node capacity longer than needed.)

## Cleanup (do this now, not later)

```
kubectl delete deployment web
kubectl delete service web
```

Then complete the mandatory checklist, `06-cleanup-checklist.md`, before moving to the next topic.

# Topic 7 / Local A: Installing kind and Creating Your First Cluster

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Topic 6 complete, kind and kubectl installed (see ../../SETUP-UBUNTU-VSCODE.md).

Install kind and stand up your first local Kubernetes cluster.

## Step 1: Confirm tools are installed

```
kind version
kubectl version --client
```


## Step 2: Create your first cluster

```
kind create cluster --name learning
```


## Step 3: Confirm kubectl is pointed at it

```
kubectl cluster-info --context kind-learning
```


## Step 4: See the node

```
kubectl get nodes
```

A default kind cluster has one node acting as BOTH control plane and worker - unlike the shared EKS cluster you looked at in Topic 6, which had separate managed control plane + worker nodes.


## Sanity checks

`kubectl get nodes` shows one Ready node named something like learning-control-plane.

Common places beginners get stuck: Forgetting which cluster/context you're pointed at once you also have EKS access (Run `kubectl config current-context` any time commands don't behave as expected.)

## Cleanup

```
# Cluster stays up for the rest of this topic's labs - no cleanup yet.
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

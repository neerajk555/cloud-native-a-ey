# Topic 6 / Local B: Pods, Nodes, and the Control Plane

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Local A complete.

Build a clear mental model of the three core pieces of any Kubernetes cluster.

## Step 1: Read: nodes and Pods


A node is one machine (real or virtual) in the cluster. A Pod is the smallest deployable unit — usually one container, wrapped with a stable network identity. Most Pods have exactly one container.


## Step 2: Read: the control plane


The control plane includes the API server (front door for all commands, including kubectl), the scheduler (decides which node a new Pod goes on), and the controller manager (continuously reconciles actual state with desired state).


## Step 3: Sketch it


Draw (on paper or in a text file) a simple diagram: one box for the control plane, three boxes for nodes, with a few Pods distributed across the node boxes.


## Sanity checks

- You can point at your own sketch and correctly explain what each box represents.

Common places beginners get stuck: Saying 'container' when you mean 'Pod' (Kubernetes schedules Pods, not individual containers directly.)

## Cleanup

```
# Nothing to clean up.
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

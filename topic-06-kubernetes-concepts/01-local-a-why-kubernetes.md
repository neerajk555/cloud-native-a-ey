# Topic 6 / Local A: Why Kubernetes Exists

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Topic 5 complete.

Understand the specific limitation of Compose that Kubernetes solves, before learning any Kubernetes commands.

## Step 1: Read: what Compose can't do


Compose runs everything on ONE Docker host. If that machine dies, everything on it goes down. There's no built-in way to spread containers across multiple machines, or to automatically move a workload if one machine fails.


## Step 2: Read: what Kubernetes adds


Kubernetes manages a cluster of machines as one logical pool. You describe what you want running ("3 copies of this app, always"), and Kubernetes continuously works to make reality match — moving workloads to healthy machines if one fails, and rolling out updates without downtime.


## Step 3: Write it in your own words


In a text file or notebook, write 2-3 sentences explaining to a colleague who only knows Compose why you'd reach for Kubernetes instead.


## Sanity checks

- You can explain, without notes, the ONE core limitation of Compose that motivates using Kubernetes.

Common places beginners get stuck: Assuming Kubernetes is just 'Compose but fancier' (It's built for a fundamentally different scale of problem — multiple machines, not one.)

## Cleanup

```
# Nothing to clean up - this was a reading/writing exercise.
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

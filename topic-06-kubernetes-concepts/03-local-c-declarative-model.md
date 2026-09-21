# Topic 6 / Local C: Declarative vs. Imperative, and the Core Objects Glossary

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Local B complete.

Understand WHY Kubernetes YAML describes desired state rather than a list of commands, and learn the vocabulary you'll use starting next topic.

## Step 1: Read: the declarative model


A Kubernetes Deployment YAML says 'there should ALWAYS be 3 replicas of this Pod running' — not a one-time command. If a Pod dies, Kubernetes immediately starts a replacement to get back to 3, without you doing anything. This is 'self-healing,' and it only works because the model is declarative and continuously reconciled.


## Step 2: Read: the glossary


- **Pod** - smallest deployable unit, usually one container
- **Deployment** - manages a set of identical Pods; handles scaling and rolling updates
- **Service** - a stable network address routing to a changing set of Pods
- **Namespace** - a way to logically divide a cluster
- **ConfigMap** - non-sensitive configuration injected into Pods
- **Secret** - like a ConfigMap but for sensitive values (base64-encoded, not encrypted by default)
- **Label / Selector** - tags on objects, and queries that match them — this is HOW a Service finds its Pods


## Sanity checks

- Without looking back, write a one-sentence definition of all 7 glossary terms from memory.

Common places beginners get stuck: Assuming a Secret is automatically encrypted (It's base64-ENCODED, trivially reversible — you'll prove this yourself in Topic 10.)

## Cleanup

```
# Nothing to clean up.
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

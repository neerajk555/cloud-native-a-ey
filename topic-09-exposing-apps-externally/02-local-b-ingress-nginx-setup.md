# Topic 9 / Local B: Installing ingress-nginx on kind

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Local A complete.

Correctly set up a kind cluster for Ingress - the extra port-mapping step almost everyone forgets the first time.

## Step 1: Recreate your cluster WITH required port mappings

```
kind delete cluster --name learning
cat > ~/course/kind-ingress.yaml <<'EOF'
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
    extraPortMappings:
      - containerPort: 80
        hostPort: 80
      - containerPort: 443
        hostPort: 443
EOF
kind create cluster --name learning --config ~/course/kind-ingress.yaml
```


**Using VS Code instead:** rather than the heredoc above, you can do this directly in the editor.

In VS Code's Explorer, create a new file named `~/course/kind-ingress.yaml`, paste this, and save (`Ctrl+S`):

```
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
    extraPortMappings:
      - containerPort: 80
        hostPort: 80
      - containerPort: 443
        hostPort: 443
```

Without extraPortMappings, ingress-nginx installs fine but is never reachable from your host - the #1 beginner mistake with Ingress on kind.


## Step 2: Install ingress-nginx

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```


## Step 3: Wait for it to be ready

```
kubectl wait --namespace ingress-nginx --for=condition=ready pod --selector=app.kubernetes.io/component=controller --timeout=120s
```


## Step 4: Confirm it's reachable

```
curl -I http://localhost
```

Any HTTP response (even a 404) confirms the controller is reachable - a 404 just means no Ingress rule matches yet.


## Sanity checks

- curl -I returns an HTTP response instead of a connection error.

Common places beginners get stuck: Using a plain `kind create cluster` for Ingress work (Always use the config file with extraPortMappings shown above.)

## Cleanup

```
# Cluster stays up for the rest of this topic.
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

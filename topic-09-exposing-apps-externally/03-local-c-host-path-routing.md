# Topic 9 / Local C: Host- and Path-Based Routing

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Local B complete.

Route traffic to different backends based on hostname AND URL path, through one Ingress.

## Step 1: Deploy two backends

```
kubectl create deployment app-a --image=hashicorp/http-echo -- -text='response from app-a'
kubectl create deployment app-b --image=hashicorp/http-echo -- -text='response from app-b'
kubectl expose deployment app-a --port=5678
kubectl expose deployment app-b --port=5678
```


## Step 2: Create a host-based Ingress

```
cat > ~/course/k8s-demo/ingress-host.yaml <<'EOF'
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: host-based
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
    - host: a.local
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: app-a
                port:
                  number: 5678
    - host: b.local
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: app-b
                port:
                  number: 5678
EOF
kubectl apply -f ~/course/k8s-demo/ingress-host.yaml
```


**Using VS Code instead:** rather than the heredoc above, you can do this directly in the editor.

In VS Code's Explorer, create a new file named `~/course/k8s-demo/ingress-host.yaml`, paste this, and save (`Ctrl+S`):

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: host-based
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
    - host: a.local
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: app-a
                port:
                  number: 5678
    - host: b.local
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: app-b
                port:
                  number: 5678
```


## Step 3: Test with Host headers

```
curl -H 'Host: a.local' http://localhost/
curl -H 'Host: b.local' http://localhost/
```


## Sanity checks

- The two curl commands return different responses despite hitting the same localhost address.

Common places beginners get stuck: Trying to open a.local in a browser without an /etc/hosts entry (Browsers don't let you set custom Host headers easily - curl with -H is the simplest way to test this locally.)

## Cleanup

```
kubectl delete -f ~/course/k8s-demo/ingress-host.yaml
kubectl delete deployment,service app-a app-b
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

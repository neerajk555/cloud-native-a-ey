# Topic 9 / AWS: Expose a Service via a Real ALB

The same exercise as the local version, now done for real on AWS using your assigned IAM user credentials -
about 15 minutes. Estimated cost: ~$0.02-0.03 (ALB bills hourly; a few minutes of use is fractions of a cent, but MUST be deleted promptly - ALBs are one of the few resources that keep billing per-hour even when idle), and only accurate if you complete the cleanup step below
right after finishing - don't leave this running "to come back to later." Prerequisites: Topic 9 Local A-C complete, Topic 8's AWS exercise complete, AWS Load Balancer Controller already installed cluster-wide by your instructor.

Expose a Deployment externally through a real AWS Application Load Balancer, using an Ingress - the same object type as your local ingress-nginx exercises, just pointed at AWS's own controller and ingress class instead of nginx's. This is the same concept you just practiced locally - the goal here is seeing how it plays out
under real AWS constraints (IAM permissions, tagging requirements, shared-account limits), not relearning the
concept itself.

## Step 1: Create a Deployment and a ClusterIP Service in your namespace

```
kubectl create deployment web --image=nginx:1.27
kubectl expose deployment web --port=80
```

Note this stays a plain ClusterIP Service - unlike a Network Load Balancer, an ALB is created by an Ingress routing to a ClusterIP Service, not by changing the Service's own type.


## Step 2: Create an Ingress using AWS's ALB ingress class, WITH required tags

```
cat > web-ingress.yaml <<'EOF'
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: web
  annotations:
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/target-type: ip
    alb.ingress.kubernetes.io/tags: Owner=$PARTICIPANT,Course=cloudnative-course-2026
spec:
  ingressClassName: alb
  rules:
    - http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: web
                port:
                  number: 80
EOF
kubectl apply -f web-ingress.yaml
```


**Using VS Code instead:** rather than the heredoc above, you can do this directly in the editor.

In VS Code's Explorer, create a new file named `web-ingress.yaml`, paste this, and save (`Ctrl+S`):

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: web
  annotations:
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/target-type: ip
    alb.ingress.kubernetes.io/tags: Owner=$PARTICIPANT,Course=cloudnative-course-2026
spec:
  ingressClassName: alb
  rules:
    - http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: web
                port:
                  number: 80
```

Without the alb.ingress.kubernetes.io/tags annotation, the ALB is created UNTAGGED - the AWS Load Balancer Controller creates it under its own separate IAM role, not your IAM identity, so your permission boundary has no say over whether this specific call succeeds. What you lose by skipping the tag is cost attribution and cleanup verification: the ALB won't show up in tag-based cost tracking, and Topic 9's own cleanup checklist's tag-based double-check would show a false "all clean" even though the ALB is still running and billing. ingressClassName: alb is what tells the AWS Load Balancer Controller (not ingress-nginx) to handle this Ingress.


## Step 3: Wait for the ALB address to appear (can take 1-2 minutes)

```
kubectl get ingress web -w
```

Ctrl+C once the ADDRESS column shows a real hostname instead of being empty.


## Step 4: Test it

```
curl http://<paste-the-ADDRESS-value-here>
```


## Sanity checks

- curl against the ALB's address returns nginx's welcome page.

Common places beginners get stuck: Using a Service annotation instead of an Ingress (A Service of type LoadBalancer with the aws-load-balancer-type annotation provisions a Network Load Balancer (Layer 4), not an ALB (Layer 7) - a genuine ALB always comes from an Ingress resource, which is also why this maps directly to what you already did with ingress-nginx locally.) Forgetting the alb.ingress.kubernetes.io/tags annotation (The controller creates the ALB under its own IAM role either way, so this doesn't get denied - but you lose cost attribution and the tag-based cleanup checklist would show a false all-clear even though the ALB is still running. The 4-hourly sweep still catches and deletes it regardless, since it doesn't rely on tags.) Leaving the ALB running after the exercise (ALBs bill hourly regardless of traffic - this is the single most expensive thing per-minute in this entire course; delete it the moment you're done verifying it works.)

## Cleanup (do this now, not later)

```
kubectl delete -f web-ingress.yaml
kubectl delete deployment web
kubectl delete service web
```

Then complete the mandatory checklist, `05-cleanup-checklist.md`, before moving to the next topic.

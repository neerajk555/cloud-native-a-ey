# Topic 09: Exposing Apps Externally

A ClusterIP Service is internal-only. Giving every app its own external LoadBalancer gets expensive and doesn't let you route by hostname/path. Ingress is a single entry point that routes external traffic to MANY Services based on hostname or URL path. On AWS, this is realized as an Application Load Balancer (ALB), created automatically from a Kubernetes Service annotation.

**How this topic is organized:** Local exercises: port-forward for quick testing, then Ingress concepts and host/path routing on kind. AWS exercise: expose a Service via a real ALB, with the tagging requirements your account's permission boundary enforces. Work through the local exercises fully before touching AWS -
they're free, fast to repeat, and build the exact muscle memory the AWS exercise assumes you already have.

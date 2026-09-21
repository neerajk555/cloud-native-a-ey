# Topic 10: Config and Secrets

ConfigMaps and Secrets inject configuration into Pods without baking it into the image - the Kubernetes equivalent of the environment variables and .env files from Topic 3. Secrets are base64-ENCODED, not encrypted, by default - anyone with API read access can trivially decode them.

**How this topic is organized:** Local exercises: ConfigMap and Secret injection, proving base64 isn't real security. AWS exercise: the same pattern on your EKS namespace, compared briefly to AWS Secrets Manager as the more secure alternative for real sensitive values. Work through the local exercises fully before touching AWS -
they're free, fast to repeat, and build the exact muscle memory the AWS exercise assumes you already have.

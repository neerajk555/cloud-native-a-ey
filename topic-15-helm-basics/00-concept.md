# Topic 15: Helm Basics

Real apps often need 5-10+ YAML files that mostly repeat with small variations between environments. Helm packages all of that into a reusable, parameterized 'chart' plus a values.yaml holding the environment-specific bits - one chart can deploy to dev, staging, and prod with different values instead of duplicated YAML.

**How this topic is organized:** Local exercises: install a public chart, scaffold your own, templating, package/upgrade/rollback - all on kind. AWS exercise: deploy your own chart to your EKS namespace, proving the same chart genuinely works across environments (the whole point of Helm). Work through the local exercises fully before touching AWS -
they're free, fast to repeat, and build the exact muscle memory the AWS exercise assumes you already have.

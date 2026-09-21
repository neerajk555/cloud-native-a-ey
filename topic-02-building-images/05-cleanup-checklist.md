# Topic 02 Cleanup Checklist: Building Images

Mandatory - do not start the next topic's AWS exercise until every box below is checked. This is how the
shared training account's cost stays predictable; skipped cleanup is the number one cause of budget overruns.

- [ ] ECR repository `myapp-$PARTICIPANT` deleted (`aws ecr describe-repositories --region us-east-1` no longer lists it)
- [ ] Local Docker images from this topic removed (`docker images | grep myapp` shows nothing)
- [ ] Ran the check below and confirmed nothing remains

```
aws resourcegroupstaggingapi get-resources --tag-filters Key=Owner,Values=$PARTICIPANT --region us-east-1
```

If that still shows something you created in this topic, delete it before continuing. Note: this generic
check only sees resources that actually carry an Owner tag - directly-created resources like Lambda
functions, ECR repos, and CodeBuild projects reliably do, but Kubernetes-provisioned resources (like the
EBS volumes behind a PVC) aren't guaranteed to inherit it the same way. Where a topic's own checklist item
above gives a more specific check (like Topic 12's volume-ID verification), trust that one first.

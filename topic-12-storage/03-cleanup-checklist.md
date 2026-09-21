# Topic 12 Cleanup Checklist: Storage

Mandatory - do not start the next topic's AWS exercise until every box below is checked. This is how the
shared training account's cost stays predictable; skipped cleanup is the number one cause of budget overruns.

- [ ] PVC `my-pvc` deleted from your namespace (`kubectl get pvc` shows nothing)
- [ ] **Explicitly verified** the underlying EBS volume is gone by re-running `aws ec2 describe-volumes --volume-ids $VOLUME_ID --region us-east-1` (the ID you captured in the exercise itself) and confirming it now errors with `InvalidVolume.NotFound` - this is more reliable than a tag-based check for this specific resource type
- [ ] If you no longer have $VOLUME_ID, list ALL available (unattached) volumes instead and check for anything unexpected: `aws ec2 describe-volumes --filters Name=status,Values=available --region us-east-1`
- [ ] If any volume DOES still show up, delete it directly: `aws ec2 delete-volume --volume-id <id> --region us-east-1`
- [ ] Ran the check below and confirmed nothing remains

```
aws resourcegroupstaggingapi get-resources --tag-filters Key=Owner,Values=$PARTICIPANT --region us-east-1
```

If that still shows something you created in this topic, delete it before continuing. Note: this generic
check only sees resources that actually carry an Owner tag - directly-created resources like Lambda
functions, ECR repos, and CodeBuild projects reliably do, but Kubernetes-provisioned resources (like the
EBS volumes behind a PVC) aren't guaranteed to inherit it the same way. Where a topic's own checklist item
above gives a more specific check (like Topic 12's volume-ID verification), trust that one first.

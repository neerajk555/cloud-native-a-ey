# Topic 12 / AWS: The Same PVC, Now Backed by a Real EBS Volume

The same exercise as the local version, now done for real on AWS using your assigned IAM user credentials -
about 15 minutes. Estimated cost: ~$0.01-0.02 for a few minutes of a 1Gi gp3 volume - BUT this keeps billing per GB-month until deleted, unlike anything else so far in this course, and only accurate if you complete the cleanup step below
right after finishing - don't leave this running "to come back to later." Prerequisites: Topic 12 Local A complete, Topic 8's AWS exercise complete.

Repeat the exact PVC pattern from Local A on your EKS namespace, and see that it provisions a real, separately-billed AWS EBS volume - then delete it immediately after confirming it works. This is the same concept you just practiced locally - the goal here is seeing how it plays out
under real AWS constraints (IAM permissions, tagging requirements, shared-account limits), not relearning the
concept itself.

## Step 1: Apply the identical PVC/Pod YAML from Local A, in your namespace

```
kubectl apply -f ~/course/k8s-demo/pvc.yaml
kubectl apply -f ~/course/k8s-demo/pod-with-pvc.yaml
```


## Step 2: Confirm a REAL EBS volume was created for this

```
kubectl get pvc my-pvc
VOLUME_ID=$(kubectl get pv $(kubectl get pvc my-pvc -o jsonpath='{.spec.volumeName}') -o jsonpath='{.spec.csi.volumeHandle}')
aws ec2 describe-volumes --volume-ids $VOLUME_ID --region us-east-1 --query 'Volumes[0].{State:State,Size:Size,VolumeType:VolumeType}'
```

This is a real AWS resource, distinct from anything on your kind cluster - it exists and bills independently of your Pod's lifecycle.


## Step 3: Write and verify data, same as Local A

```
kubectl exec pvc-demo -- sh -c 'echo persisted-data > /data/file.txt'
kubectl delete pod pvc-demo
kubectl apply -f ~/course/k8s-demo/pod-with-pvc.yaml
kubectl exec pvc-demo -- cat /data/file.txt
```


## Sanity checks

- The AWS CLI describe-volumes call returns a real volume ID with State: in-use, and your data survives Pod deletion exactly as it did locally.

Common places beginners get stuck: Deleting only the Pod and considering the exercise 'cleaned up' (The PVC (and its underlying EBS volume) survives Pod deletion BY DESIGN - you must explicitly delete the PVC too, or the EBS volume keeps billing indefinitely.) Forgetting this is the one topic where 'nothing running' doesn't mean 'nothing billing' (An EBS volume with zero Pods attached still costs money every month until deleted - this is the most important cleanup in the entire course to get right.) Verifying cleanup by tag instead of by the specific volume ID (EBS volumes created this way aren't guaranteed to carry an Owner tag the same way directly-created AWS resources do - the reliable check is the exact $VOLUME_ID you captured in Step 2, not a tag filter.)

## Cleanup (do this now, not later)

```
kubectl delete -f ~/course/k8s-demo/pod-with-pvc.yaml
kubectl delete -f ~/course/k8s-demo/pvc.yaml
# Verify the SPECIFIC volume from Step 2 is gone - this is more reliable than a tag filter,
# since dynamically-provisioned volumes aren't guaranteed to inherit an Owner tag the way
# directly-created resources do:
sleep 15
aws ec2 describe-volumes --volume-ids $VOLUME_ID --region us-east-1
```

Then complete the mandatory checklist, `03-cleanup-checklist.md`, before moving to the next topic.

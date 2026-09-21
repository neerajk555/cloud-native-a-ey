# Instructor-Only: EKS Cluster Lifecycle

Not participant-facing. Participants never create or delete the shared cluster — this is intentional (cost and
quota control across the whole cohort; see `AWS-BUDGET-GUARDRAILS.md`).

**Policy: the shared cluster is created fresh for each cohort/course run, and fully torn down at the end.** It
does not persist between cohorts.

## Before the cohort starts (before Topic 6/7)

1. **Create the cluster**, tagged for this specific cohort:
   ```bash
   eksctl create cluster --name course-shared-cluster --region us-east-1 \
     --nodes 6 --nodes-min 6 --nodes-max 12 --node-type t3.micro \
     --vpc-nat-mode Disable \
     --tags Owner=instructor,Course=cloudnative-course,Cohort=<cohort-id>
   ```
   `t3.micro` nodes are tight (~1 vCPU/1GB) — this is deliberate, matching the account's permission boundary,
   which denies larger instance types. Expect the cluster to take a few extra minutes to report all nodes Ready.

2. **Enable the OIDC provider** (only needed if any topic in your syllabus uses IRSA — not required for the
   18-topic course as currently scoped, but harmless to enable):
   ```bash
   eksctl utils associate-iam-oidc-provider --cluster course-shared-cluster --approve
   ```

3. **Create the 9 shared execution roles**, if they don't already exist account-wide (these typically outlive
   individual clusters, since they're IAM objects, not cluster objects — check first):
   ```bash
   aws iam get-role --role-name course-lambda-basic-execution 2>/dev/null || echo "Need to create shared roles - see aws-training-setup/scripts/create_shared_execution_roles.ps1"
   ```

4. **Create one namespace per participant**, from your current roster:
   ```bash
   while IFS=$'\t' read -r username full_name; do
     [ "$username" = "participant_username" ] && continue
     ns="ns-$username"
     kubectl create namespace "$ns"
     kubectl label namespace "$ns" owner="$username"
     cat <<EOF | kubectl apply -f -
   apiVersion: v1
   kind: ResourceQuota
   metadata:
     name: quota
     namespace: $ns
   spec:
     hard:
       requests.cpu: "1"
       requests.memory: 1Gi
       limits.cpu: "2"
       limits.memory: 2Gi
       pods: "10"
   EOF
   done < participants.csv
   ```

5. **Set up RBAC** so each participant can only access their own namespace — bind each participant's IAM user
   (via `aws-auth` ConfigMap mapping) to a Role scoped to their own `ns-<username>` namespace only.

6. **Install the AWS Load Balancer Controller** cluster-wide, needed for Topic 9's ALB exercise.

7. Confirm Topic 6's AWS exploration exercise works for a test user before opening the cohort.

## During the cohort

- Monitor overall node capacity — if participants collectively hit the cluster's node limits (12 nodes max in
  the config above), you may need to raise `--nodes-max` and add nodes.
- Spot-check namespaces periodically for abandoned resources participants forgot to clean up — this is a
  backstop, not a replacement for the participant-facing cleanup checklists.

## After the cohort ends (mandatory, every time)

1. Confirm every participant's namespace is empty (or don't bother checking individually — deleting the
   cluster removes everything regardless):
   ```bash
   kubectl get all --all-namespaces | grep -v kube-system
   ```
2. **Delete the cluster entirely**:
   ```bash
   eksctl delete cluster --name course-shared-cluster --region us-east-1
   ```
3. **Verify no orphaned resources survive the cluster deletion** — this is the step most likely to be skipped
   and most likely to cost money if it is:
   ```bash
   # EBS volumes from Topic 12 sometimes survive cluster deletion if not explicitly cleaned up beforehand
   aws ec2 describe-volumes --filters Name=tag:Course,Values=cloudnative-course --region us-east-1
   # Load balancers from Topic 9 should be gone if participants followed their cleanup checklists, but verify
   aws elbv2 describe-load-balancers --region us-east-1 --query "LoadBalancers[?contains(LoadBalancerName, 'k8s')]"
   ```
4. Delete anything the checks above still show.
5. The 9 shared IAM execution roles and the 2 managed policies (boundary + grant) are account-level, not
   cluster-level — leave them in place for the next cohort unless you're decommissioning the account entirely.

## Next cohort

Repeat from step 1. A fresh cluster per cohort means no cross-cohort drift in namespaces, quotas, or RBAC —
worth the ~15 minutes of setup time at the start of each run.

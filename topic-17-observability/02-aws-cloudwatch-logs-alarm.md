# Topic 17 / AWS: CloudWatch Logs and a Basic Alarm

The same exercise as the local version, now done for real on AWS using your assigned IAM user credentials -
about 15 minutes. Estimated cost: ~$0.00-0.01 (a handful of log events and one alarm, both far under free-tier limits), and only accurate if you complete the cleanup step below
right after finishing - don't leave this running "to come back to later." Prerequisites: Topic 17 Local A complete, Topic 3's Lambda exercise complete (or create a fresh small Lambda for this).

See your app's logs flow automatically into CloudWatch, and set one alarm that would notify you of a real problem. This is the same concept you just practiced locally - the goal here is seeing how it plays out
under real AWS constraints (IAM permissions, tagging requirements, shared-account limits), not relearning the
concept itself.

## Step 1: Invoke your Lambda from Topic 3 a few times to generate log data

```
for i in 1 2 3; do aws lambda invoke --function-name msg-$PARTICIPANT --region us-east-1 out.json; done
```

If you already cleaned up that function, recreate it quickly using Topic 3's AWS exercise steps first.


## Step 2: View its logs in CloudWatch

```
aws logs tail /aws/lambda/msg-$PARTICIPANT --region us-east-1
```


## Step 3: Create a basic alarm on its error count

```
aws cloudwatch put-metric-alarm --alarm-name errors-$PARTICIPANT --metric-name Errors --namespace AWS/Lambda --dimensions Name=FunctionName,Value=msg-$PARTICIPANT --statistic Sum --period 300 --threshold 1 --comparison-operator GreaterThanOrEqualToThreshold --evaluation-periods 1 --region us-east-1
```


## Step 4: Confirm the alarm exists

```
aws cloudwatch describe-alarms --alarm-names errors-$PARTICIPANT --region us-east-1 --query 'MetricAlarms[0].StateValue'
```


## Sanity checks

- The logs tail command shows your invocations' output, and the alarm shows a valid state (OK or INSUFFICIENT_DATA - either is fine, since you haven't triggered a real error).

Common places beginners get stuck: Expecting the alarm to fire immediately (It only evaluates every 5 minutes (the period you set) and needs an actual error to cross the threshold - this exercise is about SETTING UP monitoring, not necessarily seeing it fire.)

## Cleanup (do this now, not later)

```
aws cloudwatch delete-alarms --alarm-names errors-$PARTICIPANT --region us-east-1
aws lambda delete-function --function-name msg-$PARTICIPANT --region us-east-1 2>/dev/null || true
```

Then complete the mandatory checklist, `03-cleanup-checklist.md`, before moving to the next topic.

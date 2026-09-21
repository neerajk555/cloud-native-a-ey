# Topic 14 / Local A: Jobs and CronJobs

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Topic 8 complete.

Run a one-off batch task and a scheduled recurring task.

## Step 1: Create a Job

```
kubectl create job hello-job --image=busybox -- echo 'batch task complete'
kubectl get jobs -w
```

Ctrl+C once COMPLETIONS shows 1/1.


## Step 2: See its output

```
kubectl logs job/hello-job
```


## Step 3: Create a CronJob running every minute

```
kubectl create cronjob minute-task --image=busybox --schedule='* * * * *' -- echo 'tick'
```


## Step 4: Wait ~2 minutes and see the Jobs it spawned

```
sleep 120
kubectl get jobs
```


## Sanity checks

- You see hello-job completed once, and 1-2 new minute-task-<timestamp> Jobs created automatically by the CronJob.

Common places beginners get stuck: Using a Deployment for a one-off script (It would keep restarting the script forever, since Deployments expect continuous running - use a Job instead.)

## Cleanup

```
kubectl delete job hello-job
kubectl delete cronjob minute-task
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

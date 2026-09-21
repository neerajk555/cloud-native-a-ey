# Topic 15 / Local B: Scaffolding and Templating Your Own Chart

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Local A complete.

Generate your own chart, add a custom value, and reference it in a template.

## Step 1: Generate a new chart

```
cd ~/course
helm create mychart
find mychart -type f | sort
```


## Step 2: Add a custom value

```
cd mychart
echo 'greeting: hello-from-helm' >> values.yaml
```


## Step 3: Reference it in the Deployment template (edit templates/deployment.yaml)


Add this under the container spec:

```yaml
          env:
            - name: GREETING
              value: {{ .Values.greeting | quote }}
```


## Step 4: Render locally to check your work before installing

```
helm template . | grep -A2 GREETING
```


## Step 5: Install and verify

```
helm install templating-test .
kubectl exec $(kubectl get pods -l app.kubernetes.io/instance=templating-test -o jsonpath='{.items[0].metadata.name}') -- env | grep GREETING
```


## Sanity checks

- The Pod's environment shows GREETING=hello-from-helm.

Common places beginners get stuck: Skipping helm template and going straight to install (Catching a YAML/templating mistake locally is much easier than after a failed cluster deployment.)

## Cleanup

```
helm uninstall templating-test
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

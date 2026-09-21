# Topic 5 / Local B: Service-to-Service Networking by Name

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Local A complete.

See Compose's automatic DNS-by-service-name in action — the same concept Kubernetes Services will give you in Topic 8.

## Step 1: Define two services

```
cd ~/course/compose-demo
cat > compose.yaml <<'EOF'
services:
  web:
    image: nginx
  tester:
    image: alpine
    command: sh -c "apk add --no-cache curl >/dev/null && sleep 3 && curl -s http://web:80 | head -3"
EOF
```


**Using VS Code instead:** rather than the heredoc above, you can do this directly in the editor.

In VS Code's Explorer, create a new file named `compose.yaml`, paste this, and save (`Ctrl+S`):

```
services:
  web:
    image: nginx
  tester:
    image: alpine
    command: sh -c "apk add --no-cache curl >/dev/null && sleep 3 && curl -s http://web:80 | head -3"
```


## Step 2: Bring it up and watch tester reach web BY NAME

```
docker compose up
```


## Sanity checks

- tester's output shows nginx's HTML, fetched using just the hostname `web` — no IP address anywhere.

Common places beginners get stuck: Trying to use localhost to reach another service (localhost inside a container means THAT container, not another service — always use the service name.)

## Cleanup

```
docker compose down
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

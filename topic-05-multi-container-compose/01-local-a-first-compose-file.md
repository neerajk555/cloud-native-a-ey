# Topic 5 / Local A: Your First compose.yaml

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Topic 3 complete.

Write and run your first Compose file, and understand the direct mapping to `docker run` flags you already know.

## Step 1: Write the file

```
mkdir -p ~/course/compose-demo && cd ~/course/compose-demo
cat > compose.yaml <<'EOF'
services:
  web:
    image: nginx
    ports:
      - "8080:80"
EOF
```


**Using VS Code instead:** rather than the heredoc above, you can do this directly in the editor.

In VS Code's Explorer, create a new file named `compose.yaml`, paste this, and save (`Ctrl+S`):

```
services:
  web:
    image: nginx
    ports:
      - "8080:80"
```


## Step 2: Bring it up

```
docker compose up -d
```


## Step 3: See what Compose created

```
docker compose ps
```


## Step 4: Tear it down

```
docker compose down
```


## Sanity checks

- curl http://localhost:8080 works while it's up, and `docker compose ps` shows nothing after `down`.

Common places beginners get stuck: Forgetting -d and wondering why the terminal seems frozen (Without -d, Compose runs in the foreground — Ctrl+C or add -d.)

## Cleanup

```
docker compose down 2>/dev/null || true
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

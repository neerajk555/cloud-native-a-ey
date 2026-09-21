# Topic 5 / Local C: Three-Tier App: Web + API + Database

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Local A and B complete.

Combine everything from this topic into one realistic multi-service app.

## Step 1: Set up a minimal API

```
cd ~/course/compose-demo
mkdir -p api
cat > api/index.js <<'EOF'
const http = require('http');
http.createServer((req, res) => res.end(JSON.stringify({status: 'up', db: process.env.DB_HOST}))).listen(4000);
EOF
cat > api/Dockerfile <<'EOF'
FROM node:20-slim
WORKDIR /app
COPY index.js .
CMD ["node", "index.js"]
EOF
```


**Using VS Code instead:** rather than the heredoc above, you can do this directly in the editor.

In VS Code's Explorer, create a new file named `api/index.js`, paste this, and save (`Ctrl+S`):

```
const http = require('http');
http.createServer((req, res) => res.end(JSON.stringify({status: 'up', db: process.env.DB_HOST}))).listen(4000);
```

In VS Code's Explorer, create a new file named `api/Dockerfile`, paste this, and save (`Ctrl+S`):

```
FROM node:20-slim
WORKDIR /app
COPY index.js .
CMD ["node", "index.js"]
```


## Step 2: Write the full compose.yaml

```
cat > compose.yaml <<'EOF'
services:
  db:
    image: postgres:16
    environment:
      POSTGRES_PASSWORD: pw
  api:
    build: ./api
    environment:
      DB_HOST: db
    depends_on:
      - db
  web:
    image: nginx
    ports:
      - "8080:80"
    depends_on:
      - api
EOF
```


**Using VS Code instead:** rather than the heredoc above, you can do this directly in the editor.

In VS Code's Explorer, create a new file named `compose.yaml`, paste this, and save (`Ctrl+S`):

```
services:
  db:
    image: postgres:16
    environment:
      POSTGRES_PASSWORD: pw
  api:
    build: ./api
    environment:
      DB_HOST: db
    depends_on:
      - db
  web:
    image: nginx
    ports:
      - "8080:80"
    depends_on:
      - api
```


## Step 3: Bring the whole stack up

```
docker compose up -d --build
docker compose ps
```


## Step 4: Check the api tier directly

```
docker compose exec api wget -qO- http://localhost:4000
```


## Sanity checks

- All three services show as running, and the api response shows `"db":"db"` — proving it read the DB_HOST env var pointing at the db service by name.

Common places beginners get stuck: Using `image:` instead of `build:` for a custom Dockerfile (You need `build: ./api` to actually build a local image, not pull one.) Forgetting --build after changing the API's source code (`docker compose up -d` alone won't rebuild — add --build explicitly.)

## Cleanup

```
docker compose down -v
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

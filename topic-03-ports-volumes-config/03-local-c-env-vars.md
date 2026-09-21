# Topic 3 / Local C: Environment Variables and .env Files

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Topic 2 complete (myapp image).

Configure the SAME image differently at runtime, without rebuilding it.

## Step 1: Modify your app to read an env var

```
cd ~/course/myapp
cat > index.js <<'EOF'
const http = require('http');
const MSG = process.env.MESSAGE || 'default message';
http.createServer((req, res) => res.end(MSG)).listen(3000);
EOF
docker build -t myapp:envdemo .
```


**Using VS Code instead:** rather than the heredoc above, you can do this directly in the editor.

In VS Code's Explorer, create a new file named `index.js`, paste this, and save (`Ctrl+S`):

```
const http = require('http');
const MSG = process.env.MESSAGE || 'default message';
http.createServer((req, res) => res.end(MSG)).listen(3000);
```


## Step 2: Run it with a custom message

```
docker run --rm -d --name env-test -p 3001:3000 -e MESSAGE='hello from env var' myapp:envdemo
curl http://localhost:3001
```


## Step 3: Use a .env file instead

```
cat > demo.env <<'EOF'
MESSAGE=hello from a .env file
EOF
docker rm -f env-test
docker run --rm -d --name env-test -p 3001:3000 --env-file demo.env myapp:envdemo
curl http://localhost:3001
```


**Using VS Code instead:** rather than the heredoc above, you can do this directly in the editor.

In VS Code's Explorer, create a new file named `demo.env`, paste this, and save (`Ctrl+S`):

```
MESSAGE=hello from a .env file
```


## Sanity checks

- Both curl calls return the custom message you set, not 'default message'.

Common places beginners get stuck: Committing a real .env file with secrets to git (Add `.env` to `.gitignore` — this matters even more in the AWS exercise, where you'll see the cloud-native equivalent.)

## Cleanup

```
docker rm -f env-test
docker rmi myapp:envdemo
rm -f demo.env
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.

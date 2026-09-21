# Setup: Ubuntu VM + VS Code

Do this once, before Topic 1.

## 1. Install Docker

```bash
sudo apt update
curl -fsSL https://get.docker.com | sudo sh
sudo usermod -aG docker $USER
newgrp docker   # or log out and back in for the group change to take effect
docker version  # confirm both Client and Server sections appear
```

## 2. Install kubectl, kind, and Helm (needed from Topic 7 onward — fine to skip for now and come back)

```bash
# kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
rm kubectl

# kind
curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind

# Helm (needed from Topic 15 onward)
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

## 3. Install the AWS CLI (needed from Topic 2 onward)

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install -y unzip
unzip awscliv2.zip
sudo ./aws/install
rm -rf awscliv2.zip aws/
```

Configure it with the credentials your instructor gives you before Topic 2's AWS exercise:

```bash
aws configure
# Access Key ID / Secret Access Key: from your instructor
# Default region: us-east-1
# Default output format: json
```

Then, at the start of every session, export your participant variable — every AWS exercise in this course uses it:

```bash
export PARTICIPANT=$(aws iam get-user --query "User.UserName" --output text)
echo "You are: $PARTICIPANT"
```

Add that `export` line to `~/.bashrc` if you want it set automatically in every new terminal.

## 4. Install VS Code (optional, but recommended)

```bash
sudo snap install --classic code
```

Open this course's folder in it:

```bash
cd cloud-native-course   # or wherever you unzipped it
code .
```

Recommended extensions (Extensions panel, `Ctrl+Shift+X`):
- **YAML** (`redhat.vscode-yaml`) — catches indentation mistakes as you type, useful from Topic 7 onward
- **Docker** (`ms-azuretools.vscode-docker`) — build images and browse containers from a sidebar
- **Kubernetes** (`ms-kubernetes-tools.vscode-kubernetes-tools`) — cluster/resource browser, useful from Topic 7 onward

## The pattern used throughout this course

Wherever an exercise has you create a file with a command like:

```bash
cat > compose.yaml <<'EOF'
services:
  web:
    image: nginx
EOF
```

That single command creates the file and writes content into it in one step — convenient for a terminal-only
workflow. If you're using VS Code, every such step includes a **"Using VS Code instead"** box directly
underneath it, showing exactly what file to create (via the Explorer panel, right-click → New File) and what
to paste in. Both approaches produce identical results — use whichever you prefer.

## Everyday workflow once set up

- **Terminal**: `` Ctrl+` `` inside VS Code opens an integrated terminal already pointed at your project folder — every command in this course runs here exactly as written.
- **Editing files**: click a file in the Explorer to open and edit it directly, rather than retyping a whole file with a new heredoc to change one line.
- VS Code doesn't change how Docker, Kubernetes, or the AWS CLI behave — it's purely a more convenient way to create/edit files and run the same commands.

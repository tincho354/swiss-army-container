
# **Swiss Army Container**

This repository contains Dockerfiles for creating development environment containers, supporting both arm64 and amd64 architectures. These containers are equipped with a wide range of tools and utilities to facilitate development across multiple languages and platforms.

## **Contents**

- **`Dockerfile.arm64`**: Dockerfile for building the container image for the arm64 architecture.
- **`Dockerfile.amd64`**: Dockerfile for building the container image for the amd64 architecture.

## **Features**

- Base System: Ubuntu (Lunar for arm64, Jammy for amd64).
- Common development tools and utilities (vim, git, curl, wget, etc.).
- Programming environments for Go, Node.js, and Python.
- Containerization and orchestration tools (Docker, Docker Compose, Kubernetes tools).
- HashiCorp tools (Terraform, Vault, Consul, etc.).
- CLIs for various cloud services (AWS, GCP, Azure).
- Customized ZSH environment with themes and suggestions.

## **Usage**

To build the Docker images, use the following commands at the root of the repository:

For arm64:

```
docker build --build-arg TF_VERSION=0.15.6 -t swissarmycontainer:arm64 .
```

For amd64:

```
docker build --build-arg TF_VERSION=0.15.6 -t swissarmycontainer:amd64 .
```

## **Running the Container**

You can use the following generic command to run the container. Adjust the container name and tag as needed:

```
docker run -it --rm \
  --hostname swiss \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v ${PWD}:/workspace \
  -v $HOME/.ssh:/root/.ssh \
  -w /workspace \
  --name swisscontainer \
  swissarmycontainer:arm64

```

## **Setting Up an Alias for Easy Container Access**

To simplify the process of starting your container, you can set up an alias. Add the following line to your **`.bashrc`** or **`.zshrc`** file:

```
alias startswiss='docker run -it --rm \
  --hostname swiss \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v ${PWD}:/workspace \
  -v $HOME/.ssh:/root/.ssh \
  -w /workspace \
  --name swisscontainer \
  swissarmycontainer:arm64'

```

After adding this line, reload your shell configuration with the command **`source ~/.bashrc`** (or **`source ~/.zshrc`** if you're using Zsh).

Now, you can start your container simply by typing **`startswiss`** in your terminal.

This alias command will start an interactive session of your **`swissarmycontainer`** Docker container with the ARM64 architecture, mapping your current directory and SSH keys into the container for seamless development workflow.


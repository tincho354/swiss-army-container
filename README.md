
# **Swiss Army Container**

This repository contains Dockerfiles for creating development environment containers, designed to be ephemeral and support both arm64 and amd64 architectures. The --rm flag is utilized to ensure that containers are removed after use, making them ideal for one-time development tasks. These containers are equipped with a wide range of tools and utilities to facilitate development across multiple languages and platforms, providing a versatile and clean environment each time they are used.

## **Contents**

- **`Dockerfile.arm64`**: Dockerfile for image for the arm64 architecture.
- **`Dockerfile.amd64`**: Dockerfile for image for the amd64 architecture.

## **Included Tools**

Each Docker image includes a wide array of tools for diverse development needs:

- **Basic Utilities**: vim, git, curl, wget, zip, unzip, jq, tree, locate, rsync, less, file.
- **Languages & Frameworks**: Go, Node.js & npm, Python3 with pip and setuptools.
- **Containerization Tools**: Docker CLI, Docker Compose, Kubernetes tools (kubectl, kubectx, kubens, Krew).
- **HashiCorp Stack**: Terraform, Vault, Consul, Packer, Boundary, Waypoint.
- **Cloud Service CLIs**: AWS CLI, Google Cloud CLI, Azure CLI.
- **Network Tools**: ssh, netcat, iproute2, dnsutils.
- **Databases**: PostgreSQL, Redis.
- **Security Tools**: Cosign, Snyk CLI.
- **Other Utilities**: ZSH with Oh My Zsh, Powerlevel10k, Helm, Infracost, Eksctl, Calicoctl, AWS IAM Authenticator.

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
alias swiss='docker run -it --rm \
  --hostname swiss \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v ${PWD}:/workspace \
  -v $HOME/.ssh:/root/.ssh \
  -w /workspace \
  --name swisscontainer \
  swissarmycontainer:arm64'

```

After adding this line, reload your shell configuration with the command **`source ~/.bashrc`** (or **`source ~/.zshrc`** if you're using Zsh).

Now, you can start your container simply by typing **`swiss`** in your terminal.

This alias command will start an interactive session of your **`swissarmycontainer`** Docker container with the ARM64 architecture, mapping your current directory and SSH keys into the container for seamless development workflow.

## **SSH Keys Sharing**

To share your SSH keys with the container, mount your **`.ssh`** directory as a volume. For MacOS users, remember to comment out the **`keychain`** line in your SSH config if present, to ensure compatibility.

```bash
-v $HOME/.ssh:/root/.ssh
```

This command will mount your local **`.ssh`** directory to the container's **`/root/.ssh`** directory, allowing the container to use your SSH keys.

For MacOS users, if you have issues with SSH key management in the container, ensure your **`~/.ssh/config`** file is appropriately configured. Here's a common setting for MacOS:

```bash
Host github.com
  AddKeysToAgent yes
  #UseKeychain yes
  IdentityFile ~/.ssh/your_private_key
```

If you're experiencing issues, try commenting out the **`UseKeychain`** line (add a **`#`** at the beginning of the line). This change might be necessary because the keychain access doesn't work inside the Docker container.

## **Customization**

Feel free to customize the Dockerfiles according to your needs by modifying tool versions, adding or removing packages, etc.

## **Contributions**

Contributions are welcome. If you wish to add more tools or make improvements, please send a Pull Request.
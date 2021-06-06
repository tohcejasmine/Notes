# Ubuntu 20.04 VM

This document lists down the applications installed on the virtual machine. Running on virtualization software package VMware Workstation 16 Player.

Before most installations, remember to run:

```bash
sudo apt-get update
```

## Python

Python3.8.5 is already installed by default in Ubuntu 20.04.

## Postman

1. ```bash
   sudo snap install postman
   ```

## VSCode

1. 

   ```bash
   sudo snap install --classic code
   ```

## Typora

A note-taking editor that live previews markdown documents.

1. Get public key

   ```bash
   wget -qO - https://typora.io/linux/public-key.asc | sudo apt-key add -
   ```

2. Add Typora repository

   ```ba
   sudo add-apt-repository 'deb https://typora.io/linux ./'
   sudo apt-get update

3. Install  Typora

   ```bash
   sudo apt-get install typora
   ```

## Docker

1. Set up repository

   ```shell
    sudo apt-get update
    sudo apt-get install \
       apt-transport-https \
       ca-certificates \
       curl \
       gnupg \
       lsb-release
   ```

2. Add Docker's official GPG key

   ```shell
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   ```

3. Set up the **stable** repository

   ```sh
   echo \
   "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

4. Update `apt` package index

   ```shell
   sudo apt-get update
   ```

5. Install _latest_ version of Docker Engine and containerd

   ```shell
   sudo apt-get install docker-ce docker-ce-cli containerd.io
   ```

6. Verify Docker Engines is installed correctly by running the `hello-world` image

   * This commands downloads a test image and runs it in a container
   * When the container runs, it prints an informational message and exits

   ```shell
   sudo docker run hello-world
   ```

## Docker-Compose

1. Install current stable release of Docker Compose

   ```shell
   sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   ```

2. Apply executable permissions for downloaded binary

   ```shell
   sudo chmod +x /usr/local/bin/docker-compose
   ```

3. Verify Docker Compose installation

   ```shell
   docker-compose --version
   ```

## MongoDB, MongoDB Compass

Refer to setup  instructions at `\vm-notes\MongoDB`.

## `apt` packages

1. vim
2. htop
3. pip
4. tree
5. net-tools (for `ifconfig`)

```shell
sudo apt install vim htop python3-pip tree net-tools
```

## References

1. https://typora.io/#linux
2. https://docs.docker.com/engine/install/ubuntu/
3. https://www.bmc.com/blogs/mongodb-docker-container/

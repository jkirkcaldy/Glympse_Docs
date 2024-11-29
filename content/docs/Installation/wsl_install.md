---
title: Docker system setup on WSL for Windows
type: docs
next: docs/installation/services
next: docs/installation/install_glympse
---

Whilst it is possible to run docker desktop on Windows/MacOS. This may have some potential licensing costs. As such the below script will allow you to install docker and the requirements to run Glympse on Ubuntu on WSL. 

Open the windows terminal app and run the following commands. 

```powershell
# Install WSL
wsl --install
wsl --set-default-version 2
# Install Ubuntu on WSL
wsl --install -d Ubuntu
```

The following commands need to be run from within the Ubuntu system. You can access this by typing `wsl -d Ubuntu` to enter a cmd prompt.

```bash
#install the prerequisit dependancies 
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
sudo echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list


# install docker and docker compose
sudo apt update
sudo apt install -y docker-ce

sudo mkdir -p ~/.docker/cli-plugins/
sudo curl -SL https://github.com/docker/compose/releases/download/v2.3.3/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose
sudo chmod +x ~/.docker/cli-plugins/docker-compose

#Enable the nvidia runtime
sudo apt-get install -y nvidia-container-toolkit nvidia-utils-535-server
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker

#Optional step to allow you to run docker commands without root or sudo access
sudo usermod -aG docker ${USER}
su - ${USER}

```


You can check that your Nvidia GPU is working in WSL by typing `nvidia-smi`.

You are now ready to install Glympse
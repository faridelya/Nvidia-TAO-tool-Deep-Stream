# Install Tao Tool kit on PC

All below steps taken from [official doc](https://docs.nvidia.com/tao/tao-toolkit/text/tao_toolkit_quick_start_guide.html) check --> Running TAO Toolkit and follow the launcher cli.

## Step1. 

Install [docker ce](https://docs.docker.com/engine/install/ubuntu/)

## Step 2.
[Post Installation of Docker-ce --> Manage Docker as a non-root user](https://docs.docker.com/engine/install/linux-postinstall/)
1. Create the docker group.
```
sudo groupadd docker
```
2. Add your user to the docker group.
```
sudo usermod -aG docker $USER
```
3. You can also run the following command to activate the changes to groups:
```
newgrp docker
```
4. Verify that you can run docker commands without sudo.
```
docker run hello-world
```
if you got error follow official link of post installation.

## Step 3
Install nvidia-container-toolkit by following the [install-guide](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html)follow Installing on Ubuntu and Debian section.
The list of prerequisites for running NVIDIA Container Toolkit is described below:

GNU/Linux x86_64 with kernel version > 3.10

Docker >= 19.03 (recommended, but some distributions may include older versions of Docker. The minimum supported version is 1.12)

NVIDIA GPU with Architecture >= Kepler (or compute capability 3.0)

NVIDIA Linux drivers >= 418.81.07 (Note that older driver releases or branches are unsupported.)

1. Setting up NVIDIA Container Toolkit
Setup the package repository and the GPG key:
```
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
      && curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
      && curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | \
            sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
            sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```

2. Install the nvidia-docker2 package (and dependencies) after updating the package listing:
```
sudo apt-get update
```
```
sudo apt-get install -y nvidia-docker2
```
Restart the Docker daemon to complete the installation after setting the default runtime:
```
sudo systemctl restart docker
```
At this point, a working setup can be tested by running a base CUDA container:

```
sudo docker run --rm --gpus all nvidia/cuda:11.6.2-base-ubuntu20.04 nvidia-smi
```
output
```
Unable to find image 'nvidia/cuda:11.6.2-base-ubuntu20.04' locally
11.6.2-base-ubuntu20.04: Pulling from nvidia/cuda
846c0b181fff: Already exists 
b787be75b30b: Pull complete 
40a5337e592b: Pull complete 
8055c4cd4ab2: Pull complete 
a0c882e23131: Pull complete 
Digest: sha256:9928940c6e88ed3cdee08e0ea451c082a0ebf058f258f6fbc7f6c116aeb02143
Status: Downloaded newer image for nvidia/cuda:11.6.2-base-ubuntu20.04
Fri Jan 13 07:31:46 2023       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 520.61.05    Driver Version: 520.61.05    CUDA Version: 11.8     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA GeForce ...  On   | 00000000:05:00.0  On |                  N/A |
|  0%   46C    P8    11W / 170W |    243MiB / 12288MiB |      2%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
+-----------------------------------------------------------------------------+
```

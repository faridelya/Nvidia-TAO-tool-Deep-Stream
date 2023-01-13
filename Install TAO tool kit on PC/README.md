# Install Tao Tool kit on PC
TAO toolkit is available as a docker container or a collection of python wheels. There are 4 ways to run TAO Toolkit depending on your preference and setup, through

 - the launcher CLI

 - the containers directly

 - the tao toolkit apis

 - python wheels
we will use the first method ( launcher CLI ) for installation
# 1 Installing the Pre-requisites
The TAO Toolkit launcher is strictly a python3 only package, capable of running on python versions >= 3.6.9.

All the below steps taken from [official doc](https://docs.nvidia.com/tao/tao-toolkit/text/tao_toolkit_quick_start_guide.html) check --> Running TAO Toolkit and we are using the launcher cli method for installtion of tao tool kit.

## Step1. 
The given link show Ubuntu installation you can install on other distro just follow the link.
Install [docker ce](https://docs.docker.com/engine/install/ubuntu/)

## Step 2.
i will show all steps but if you want to see official doc then click post installation link.
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

## Step 4
just make account on NGC and follow other steps.
Get an [NGC](https://catalog.ngc.nvidia.com/) account and API key:

1. Go to NGC and click the **TAO Toolkit** container in the **Catalog** tab. This message is displayed: “Sign in to access the PULL feature of this repository”.
2. Enter your Email address and click Next, or click Create an Account.
3. Choose your organization when prompted for Organization/Team.

Click Sign In.

## Step 4
1. login to NGC accountand click on **Profile**-->**Setup**-->**Generate API KEY**
2. log in to the NGC docker registry (nvcr.io) using the command docker login nvcr.io and enter the following credentials:
copy and Run this command on terminal.
```
docker login nvcr.io
```
output:
 > a. Username: "$oauthtoken"
 > b. Password: "YOUR_NGC_API_KEY"
 
where YOUR_NGC_API_KEY corresponds to the key you generated from step 4 where we created account and we will generate key there.

## Step 5
NVIDIA recommends setting up a python environment using [miniconda](https://docs.conda.io/en/latest/miniconda.html) but i have already anaconda i will skip this part. but if you dont have just follow.

1. once install miniconda.
```
conda create -n launcher python=3.6
```
2. Activate the conda environment that you have just created.
```
conda activate launcher
```
3. Once you have activated your conda environment,
```
(launcher) py-3.6.9 desktop:
```
4. When you are done with you session, you may deactivate your conda environment
```
conda deactivate
```
5. You may re-instantiate this created conda environment.
```
conda activate launcher
```
# 2 Installing TAO Launcher

Once you have installed the required pre-requisites.

1. Install the CLI launcher via the quick start script downloaded with the getting_started NGC package from [here] or you can watch given below.
 - quick start script
-  1st
```
wget --content-disposition https://api.ngc.nvidia.com/v2/resources/nvidia/tao/tao-getting-started/versions/4.0.0/zip -O getting_started_v4.0.0.zip
unzip -u getting_started_v4.0.0.zip  -d ./getting_started_v4.0.0 && rm -rf getting_started_v4.0.0.zip && cd ./getting_started_v4.0.0
```
- 2nd 
```
bash setup/quickstart_launcher.sh --install
```
2. You can also use this script to update the launcher to the latest version of TAO Toolkit by running the following command
```
bash setup/quickstart_launcher.sh --upgrade
```
3. Invoke the entrypoints using the tao command.
```
tao --help
```

 > When installing the TAO Toolkit Launcher to your host machine’s native python3 as opposed to the recommended route of using virtual environment, you may get an error saying that tao binary wasn’t found. This is because the path to your tao binary installed by pip wasn’t added to the PATH environment variable in your local machine. In this case, please run the following command:
```
export PATH=$PATH:~/.local/bin
```



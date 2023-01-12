# Nvidia Deep-Stream SDK

## **Method 1.**
you can use above Scripting file for auto installation of dependences and all things for Deep Stream SDK.
All the enviroment setup commands taken [from](https://docs.nvidia.com/metropolis/deepstream/dev-guide/text/DS_Quickstart.html) under section ( dGPU Setup for Ubuntu ). 

- For runing any above file in terminal (  bash ./00_uninstall_exisiting_setup.sh ).
- If you have Nvidia driver, Cuda, CuDnn installed on PC so donn't run this **00_uninstall_exisiting_setup.sh** file. and run all other files one by one.
- The Last scripting file **05_bindings.sh** is for installation of GStream taken from [here](https://github.com/NVIDIA-AI-IOT/deepstream_python_apps/tree/master/bindings) 

For Running sample with the help of Gst Python:
  ```
  cd
  ```
  ```
  cd /opt/nvidia/deepstream/deepstream-6.1/sources/deepstream_python_apps/apps/deepstream-test1
  ```
  ```
  sudo python3 deepstream_test_1.py  /opt/nvidia/deepstream/deepstream-6.1/samples/streams/sample_720p.h264
  ```
## **Method 2.**
we will require the following packages for setting up DeepStream on PC (OS Ubuntu 20.04).
 - You must install the following components mention in **4 link**:
    - Ubuntu 20.04
    - GStreamer 1.16.2
    - NVIDIA driver 515.65.01
    - CUDA 11.7 Update 1 
    - TensorRT 8.4.1.5
    
1. Install Nvidia Driver 
 -you can install from my Deep Learining repo
2. [install Cuda version 11.8 or 11.7, i have cuda 11.8 it work](https://developer.nvidia.com/cuda-toolkit-archive)
3. [Download deb or tar file of **Deep Stream SDK**](https://developer.nvidia.com/deepstream-getting-started)
4. [Note follow this section for installation  -- **dGPU Setup for Ubuntu** ](https://docs.nvidia.com/metropolis/deepstream/dev-guide/text/DS_Quickstart.html)
5. [Installing GStream and build](https://github.com/NVIDIA-AI-IOT/deepstream_python_apps/tree/master/bindings)

For running sample with deepstream-app:
 ```
 cd
 ```
 ```
 cd /opt/nvidia/deepstream/deepstream-6.1/samples/configs/deepstream-app
 ```
 ```
 sudo deepstream-app -c source4_1080p_dec_infer-resnet_tracker_sgie_tiled_display_int8.txt
 ```
 



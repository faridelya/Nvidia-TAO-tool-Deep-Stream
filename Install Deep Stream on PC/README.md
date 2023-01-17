# Intsallation of Nvidia Deep-Stream SDK

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
 - You must install the following components mention in **4**:
    - Ubuntu 20.04
    - GStreamer 1.16.2
    - NVIDIA driver 515.65.01
    - CUDA 11.7 Update 1 
    - TensorRT 8.4.1.5
    
1. Install Nvidia Driver 
 -you can install from my Development repo.
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
 

[The above scripts are taken from this repo thanks to his effort](https://github.com/rajeshroy402/DeepStream-dGPU-Installation/tree/20.04-6.1)

# Fine Tune TAO pre-trained Model for specific downstream task.

1. [Want to learn more about TAO](https://docs.nvidia.com/tao/tao-toolkit/text/overview.html) 
1. [TAO tool kit all pre-trained models that can run on colab without effort](https://docs.nvidia.com/tao/tao-toolkit/text/running_in_cloud/running_tao_toolkit_on_google_colab.html#running-tao-toolkit-on-google-colab)
2. [Deploying to DeepStream for YOLOv4](https://docs.nvidia.com/tao/tao-toolkit/text/ds_tao/yolo_v4_ds.html#)
3. [Integrate TAO model to Deep Stream github](https://github.com/NVIDIA-AI-IOT/deepstream_tao_apps)


# Run the TAO pre-trained purpose-built models in DeepStream
click [here](https://github.com/NVIDIA-AI-IOT/deepstream_reference_apps/tree/master/deepstream_app_tao_configs) if you want to follow official doc. Given below all steps have taken from official doc you can follow along.

1. Download the config files
```
git clone https://github.com/NVIDIA-AI-IOT/deepstream_reference_apps.git
cd deepstream_reference_apps/deepstream_app_tao_configs/
sudo cp -a * /opt/nvidia/deepstream/deepstream/samples/configs/tao_pretrained_models/
```
2. Download the models
```
sudo apt install -y wget zip
cd /opt/nvidia/deepstream/deepstream/samples/configs/tao_pretrained_models/
sudo ./download_models.sh
```

3. Run the models in DeepStream
```
sudo deepstream-app -c deepstream_app_source1_$MODEL.txt
```
e.g.
```
sudo deepstream-app -c deepstream_app_source1_dashcamnet_vehiclemakenet_vehicletypenet.txt
```
The yaml config files can also be used

```
sudo deepstream-app -c deepstream_app_source1_$MODEL.yml
```
e.g.
```
sudo deepstream-app -c deepstream_app_source1_dashcamnet_vehiclemakenet_vehicletypenet.yml
```
**Note:**
 - For which model of the deepstream_app_source1_$MODEL.txt uses, please find from the [primary-gie] section in it, for example

Below is the [primary-gie] config of deepstream_app_source1_detection_models.txt, which indicates it uses yolov4 by default, and user can change to frcnn/ssd/dssd/retinanet/yolov3/detectnet_v2 by commenting "config-file=config_infer_primary_yolov4.txt" and uncommenting the corresponding "config-file=" .
```
[primary-gie]
enable=1
gpu-id=0
# Modify as necessary
batch-size=1
#Required by the app for OSD, not a plugin property
bbox-border-color0=1;0;0;1
bbox-border-color1=0;1;1;1
bbox-border-color2=0;0;1;1
bbox-border-color3=0;1;0;1
gie-unique-id=1
# Replace the infer primary config file when you need to
# use other detection models
#config-file=config_infer_primary_frcnn.txt
#config-file=config_infer_primary_ssd.txt
#config-file=config_infer_primary_dssd.txt
#config-file=config_infer_primary_retinanet.txt
#config-file=config_infer_primary_yolov3.txt
config-file=config_infer_primary_yolov4.txt
#config-file=config_infer_primary_detectnet_v2.txt
```

4. Related Links

[deepstream-tao-app :](https://github.com/NVIDIA-AI-IOT/deepstream_tao_apps)
[TAO Toolkit Guide : ](https://docs.nvidia.com/tao/tao-toolkit/index.html)


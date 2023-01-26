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
5. [Installing binding for python](https://github.com/NVIDIA-AI-IOT/deepstream_python_apps/tree/master/bindings)

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
- The above deep stream command run one yml file that ci=onatin one reference file for model configuration for Deep stream, like the above yml file has the **primary-gie** group that has refrence to **config-file** as you can see. Thsis config file has some parameter for loading modle into Deep stream which are.
- The DeepStream configuration file includes some runtime parameters for DeepStream nvinfer plugin, such as 
- model path, 
- label file path, 
- TensorRT inference precision, 
- input and output node names, 
- input dimensions and so on.

The above file show diferent config file for different models, each model has its own DeepStream configuration file.

4. Related Links
 - the following are some repository.that show how tao model integrate with deep stream app.
 1. [deepstream-tao-app :](https://github.com/NVIDIA-AI-IOT/deepstream_tao_apps) this repo show **Integrate TAO model with DeepStream SDK** but this doesnot work for me.
 2. the above shown command are from **deepstream_reference_apps** this repo also show how to run tao model with deep stream.i found this in tao_pretrained_models folder in readme file so i use this way and it work for sample as well for custom model now i will show hoe to integrated custom tao model integration with Deep stream.
[TAO Toolkit Guide : ](https://docs.nvidia.com/tao/tao-toolkit/index.html)

# Integrate Tao model ( trained on specific data ) in Deep Stream.

You must have done all above steps before doing this.
we have two category of files in deep stream app on this path
```
/opt/nvidia/deepstream/deepstream-6.1/samples/configs/tao_pretrained_models
```
1. config_infer_primary_detectnet_v2.txt or yml     ( supporting config file )
2. deepstream_app_source1_detection_models.txt or yml  ( main config file )
i just placed the trained model and other files like cache file , enginefile , model.etlt and labels.txt in the following directory path 
```
/opt/nvidia/deepstream/deepstream-6.1/samples/models/tao_pretrained_models/yolov4/n/
```
and go to configuration path and create new file and copy and paste main and supporting config to your new created files and set parameter just like i did in the following files.
- supporting config file  i named this **nvinfer_config.txt** you can rename.
 we have two options either set engine file and cache file along with model.etlt and labels.txt     but this engine file willspecific to hardware use second option.
 The second option is setup parameter for model.etlt and labels.txt and optional cahce file so deep stream will autogenerate engine file 
```
[property]
gpu-id=0
net-scale-factor=1.0
offsets=103.939;116.779;123.68
model-color-format=1
labelfile-path= ../../models/tao_pretrained_models/yolov4/n/labels.txt      # set this path to your trained tao model labels
model-engine-file=../../models/tao_pretrained_models/yolov4/n/trt.engine    # set this path to your trained tao model engine file
int8-calib-file=../../models/tao_pretrained_models/yolov4/n/cal.bin         # # set this path to your trained tao model calibiration cache
tlt-encoded-model=../../models/tao_pretrained_models/yolov4/n/yolov4_resnet18_epoch_010.etlt   # set this path to your trained tao-model.eltl
tlt-model-key = NGpmbHN0ZTNrZHFkOGRxNnFsbW9rbXNxbnU6Yzc5NWM5MjQtZDE1YS00NTYxLTg3YzgtNTU2MWVhNDg1M2M3  # set the key which you are using during training time.
infer-dims=3;384;1248   # set this infer dimension when generate config for model that will have this infor
maintain-aspect-ratio=1
uff-input-order=0
uff-input-blob-name=Input
batch-size=1
## 0=FP32, 1=INT8, 2=FP16 mode
network-mode=0
num-detected-classes=3
interval=0
gie-unique-id=1
is-classifier=0
#network-type=0
cluster-mode=3
output-blob-names=BatchedNMS
parse-bbox-func-name=NvDsInferParseCustomBatchedNMSTLT
custom-lib-path=/opt/nvidia/deepstream/deepstream/lib/libnvds_infercustomparser.so
layer-device-precision=cls/mul:fp32:gpu;box/mul_6:fp32:gpu;box/add:fp32:gpu;box/mul_4:fp32:gpu;box/add_1:fp32:gpu;cls/Reshape_reshape:fp32:gpu;box/Reshape_reshape:fp32:gpu;encoded_detections:fp32:gpu;bg_leaky_c>

[class-attrs-all]
pre-cluster-threshold=0.3
roi-top-offset=0
roi-bottom-offset=0
detected-min-w=0
detected-min-h=0
detected-max-w=0
detected-max-h=0

[class-attrs-1]
nms-iou-threshold=0.9
```
- Main config file   i name this file **deepstrean_app_source1_custom_yolov4.txt** 
```
[application]
enable-perf-measurement=1
perf-measurement-interval-sec=1

[tiled-display]
enable=1
rows=1
columns=1
width=1280
height=720
gpu-id=0

[source0]
enable=1
#Type - 1=CameraV4L2 2=URI 3=MultiURI
type=3
num-sources=1
uri=file://../../streams/sample_1080p_h265.mp4
gpu-id=0

[streammux]
gpu-id=0
batch-size=1
batched-push-timeout=40000
## Set muxer output width and height
width=1920
height=1080

[sink0]
enable=1
#Type - 1=FakeSink 2=EglSink 3=File
type=2
sync=1
source-id=0
gpu-id=0

[osd]
enable=1
gpu-id=0
border-width=3
text-size=15
text-color=1;1;1;1;
text-bg-color=0.3;0.3;0.3;1
font=Arial

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
config-file = nvinfer_config.txt                      # just set this to you supporting config file and other all will be default
#config-file=config_infer_primary_dssd.txt
#config-file=config_infer_primary_retinanet.txt
#config-file=config_infer_primary_yolov3.txt
#config-file=config_infer_primary_yolov4.txt
#config-file=config_infer_primary_detectnet_v2.txt
#config-file=config_infer_primary_yolov4-tiny.txt
#config-file=config_infer_primary_efficientdet.txt

[sink1]
enable=0
type=3
#1=mp4 2=mkv
container=1
#1=h264 2=h265 3=mpeg4
codec=1
#encoder type 0=Hardware 1=Software
enc-type=0
sync=0
bitrate=2000000
#H264 Profile - 0=Baseline 2=Main 4=High
#H265 Profile - 0=Main 1=Main10
profile=0
output-file=out.mp4
source-id=0

[sink2]
enable=0
#Type - 1=FakeSink 2=EglSink 3=File 4=RTSPStreaming 5=Overlay
type=4
#1=h264 2=h265
codec=1
#encoder type 0=Hardware 1=Software
enc-type=0
sync=0
bitrate=4000000
#H264 Profile - 0=Baseline 2=Main 4=High
#H265 Profile - 0=Main 1=Main10
profile=0
# set below properties in case of RTSPStreaming
rtsp-port=8554
udp-port=5400

[tracker]
enable=1
# For NvDCF and DeepSORT tracker, tracker-width and tracker-height must be a multiple of 32, respectively
tracker-width=640
tracker-height=384
ll-lib-file=/opt/nvidia/deepstream/deepstream/lib/libnvds_nvmultiobjecttracker.so
# ll-config-file required to set different tracker types
# ll-config-file=../deepstream-app/config_tracker_IOU.yml
ll-config-file=../deepstream-app/config_tracker_NvDCF_perf.yml
# ll-config-file=../deepstream-app/config_tracker_NvDCF_accuracy.yml
# ll-config-file=../deepstream-app/config_tracker_DeepSORT.yml
gpu-id=0
enable-batch-process=1
enable-past-frame=1
display-tracking-id=1

[tests]
file-loop=0
```

when we run deep stream with my trained tao model, we use 
```
sudo deepstream-app -c deepstrean_app_source1_custom_yolov4.txt
```
[you can also check offical doc for help](https://docs.nvidia.com/tao/tao-toolkit/text/ds_tao/yolo_v4_ds.html#integrating-yolov4-model)

---
title: "YOLO + TensorRT"
subtitle: Have you Optimized your Deep Learning Model Before Deployment?
layout: single
classes: wide
tags: [Image classification, Computer Vision]
excerpt: "Optimization of deep learning models"
header:
  image: /assets/images/tensorrt.png
  teaser: /assets/images/yolo_tensorrt/yolo_tensorrt_teaser.png
  caption: "nvidia.com"
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
author_profile: true
date: 2019-07-12
---

## 🤖 Link to the project code

- [https://gitlab.com/aminehy/YOLOv3-Darknet-ONNX-TensorRT.git](https://gitlab.com/aminehy/YOLOv3-Darknet-ONNX-TensorRT.git)

- [https://gitlab.com/aminehy/yolov3-darknet](https://gitlab.com/aminehy/yolov3-darknet)

- [https://gitlab.com/aminehy/YOLOv3-Caffe-TensorRT](https://gitlab.com/aminehy/YOLOv3-Caffe-TensorRT)


# Illustration on an AI-based Computer Vision Application

This article presents how to use NVIDIA TensorRT to optimize a deep learning model that you want to deploy on the edge device (mobile, camera, robot, car ….). For instance, the intelligent double spectrum camera of Aerialtronics: _Pensar_ <https://pensarsdk.com/>

![\[https://pensarsdk.com/\](https://pensarsdk.com/)](https://cdn-images-1.medium.com/max/2280/1*kF5CrVqanC408hSvNYO1lg.png)
_Source: <https://pensarsdk.com/>_

Which has onboard an NVIDIA Jetson TX2 GPU

![\[https://developer.nvidia.com/embedded/jetson-tx2-developer-kit\](https://developer.nvidia.com/embedded/jetson-tx2)](https://cdn-images-1.medium.com/max/2000/1*QRaU5AHVxGZeKAqwpriktg.png)
_Source: [NVIDIA Jetson](https://developer.nvidia.com/embedded/jetson-tx2)_


This article is organized as follows:

-   What is NVIDIA TensorRT?

-   Setup the Development Environment using docker

-   Computer Vision Application: Object detection with YOLOv3 model

-   References

-   Conclusion

## Why do I need an optimized deep learning model?

As an example, think of AI-based computer vision application, they need to process each frame captured by the camera. Thus, each frame makes a forward pass through the layers of the model to compute a certain output (detection, segmentation, classification…).

Whatever power your GPU has, we all want the number of frames per second (FPS) at the output to be equal to one at the input (example 24, 30 FPS…). This means that the GPU is processing each frame in real-time.

This notion is more than desired for Computer Vision applications that require a real-time decision, for instance, surveillance, fraud detection, or crowd counting during an event, to cite just a few.

## Pre-Deployment Optimization Workflow

Among tasks of a Data Scientist is to exploit the data and develop/train/test a neural network architecture. After the validation of the model, usually, the architecture and the model’s parameters are exported for deployment. Many ways are possible to do that, either on a cloud or an edge device. In this article, we focus on deployment on edge device (camera, mobile, robot, car…).

The workflow of deployment, generally speaking, follows the block diagram depicted in the figure below:

![](https://cdn-images-1.medium.com/max/2246/1*ftr5BLj4PitHbueEqS6JdQ.png)

From the exported, pre-trained, deep learning model **->** framework parser **->** TensorRT optimization **->** Inference on new data

# What Is NVIDIA TensorRT?

![\[https://docs.nvidia.com/deeplearning/sdk/tensorrt-archived/tensorrt_210/tensorrt-user-guide/\](https://docs.nvidia.com/deeplearning/sdk/tensorrt-archived/tensorrt_210/tensorrt-user-guide/index.html)](https://cdn-images-1.medium.com/max/2000/1*EWiIvB6RjwEZrlSLF1Xttw.png)_[https://docs.nvidia.com/deeplearning/sdk/tensorrt-archived/tensorrt_210/tensorrt-user-guide/](https://docs.nvidia.com/deeplearning/sdk/tensorrt-archived/tensorrt_210/tensorrt-user-guide/index.html)_

> The core of NVIDIA TensorRT is a C++ library that facilitates high-performance inference on NVIDIA graphics processing units (GPUs). TensorRT takes a trained network, which consists of a network definition and a set of trained parameters, and produces a highly optimized runtime engine which performs inference for that network.
> You can describe a TensorRT network using a C++ or Python API, or you can import an existing Caffe, ONNX, or TensorFlow model using one of the provided parsers.

TensorRT provides API’s via C++ and Python that help to express deep learning models via the Network Definition API or load a pre-defined model via the parsers that allows TensorRT to optimize and run them on a NVIDIA GPU. TensorRT applies graph optimizations, layer fusion, among other optimizations, while also finding the fastest implementation of that model leveraging a diverse collection of highly optimized kernels. TensorRT also supplies a runtime that you can use to execute this network on all of NVIDIA’s GPUs from the Kepler generation onwards.

TensorRT also includes optional high speed mixed precision capabilities introduced in the Tegra X1, and extended with the Pascal, Volta, and Turing architectures.

Learn more about how to use TensorRT in the [developer guide]([https://docs.nvidia.com/deeplearning/sdk/tensorrt-developer-guide/index.html](https://docs.nvidia.com/deeplearning/sdk/tensorrt-developer-guide/index.html)) and interaction with the TensorRT community in the [TensorRT forum](https://devtalk.nvidia.com/default/board/304/tensorrt/)

# Setup of a Development Environment using docker

## Docker

For the development, we use a docker image that contains an installed version of NVIDIA TensorRT. This makes the development environment more reliable and scalable on the different operating system (Windows, Linux, macOS).

Here is an illustration of the docker positioning between the application and the GPU.

![docker hierarchy](https://cdn-images-1.medium.com/max/2246/1*-wpy3yNzLK_T2valKYMdVw.png)

**Note:** If you never heard about ‘docker’ then I highly recommend you to invest in learning about it, you can start [here](https://docs.docker.com/engine/docker-overview/).

## Install Docker-CE

Head over to the official website of docker and follows the steps to install `docker-ce` (ce stands for community edition). I am using Ubuntu 64 bit, thus, here the installation [link](https://docs.docker.com/v17.09/engine/installation/linux/docker-ce/ubuntu/).

## Install CUDA

In addition, you should have installed the latest version of [CUDA ](https://developer.nvidia.com/cuda-downloads). It is a parallel computing platform and application programming interface model created by NVIDIA. It allows software developers and software engineers to use a CUDA-enabled GPU for general purpose processing (read more about it in [here](https://developer.nvidia.com/cuda-zone)).

To check if CUDA is correctly installed on you machine, just enter in the terminal

```bash
  nvidia-smi
```

The output should look something like that (I have a `NVIDIA GPU GeForce GTX 1660 Ti/PCIe/SSE2`)

```bash
    Thu Aug 1 10:43:37 2019
    + — — — — — — — — — — — — — — — — — — —— — — — — — — — — — — -+
    | NVIDIA-SMI 430.40 **Driver Version: **430.40 **CUDA Version: **10.1 |
    | — — — — — — — — — — — — -+ — — — — — — — — + — — — —— — — — +
    | GPU Name Persistence-M| Bus-Id Disp.A | Volatile Uncorr. ECC |
    | Fan Temp Perf Pwr:Usage/Cap| Memory-Usage | GPU-Util Compute M. |
    |=====================+===================+===================|
    | 0 GeForce GTX 166… Off | 00000000:01:00.0 Off | N/A |
    | N/A 47C P8 4W / N/A | 994MiB / 5944MiB | 8% Default |
    + — — — — —— — — — — — — -+ — — — — — — — — + — — — — — — — — +
```

## Run the docker image

I have created a docker image that includes installation of TensorRT on top of Ubuntu along with the necessary prerequisites from NVIDIA, Python, OpenCV, …. You can pull the image directly from my personal account on [docker hub](https://hub.docker.com/r/aminehy/tensorrt-opencv-python3)

-   First, open a terminal (ctrl+alt + t) and enter this command to pull the docker image

    ```bash
     docker pull aminehy/tensorrt-opencv-python3:version-1.3
    ```

-   Enable launching GUI applications from inside the docker container bu entering the command

    ```bash
     xhost +
    ```

-   Lastly, run the docker container with the command:

    ```bash
    docker run -it — rm -v $(pwd):/workspace — runtime=nvidia -w /workspace -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY aminehy/tensorrt-opencv-python3:version-1.3
    ```

# Computer Vision Application: Object Detection with YOLOv3

YOLO is a Real-Time Object Detection from the DarkNet project. You can read more about this project on the official website here: <https://pjreddie.com/darknet/yolo/>

![\[https://pjreddie.com/darknet/yolo/\](https://pjreddie.com/darknet/yolo/)](https://cdn-images-1.medium.com/max/3520/1*aFeEgXtb0ar-Zrxujmu1oA.png)
_Source: <https://pjreddie.com/darknet/yolo/>_

In this experiment, we run YOLOv3 model on 500 images and compare the average inference time before and after optimization of the model with NVIDIA TensorRT. The images used in this experiment are from COCO dataset: [COCO - Common Objects in Context](http://cocodataset.org/#home).

## 1) Running a non-optimized YOLOv3

Link to the project in gitlab: [Amine Hy / YOLOv3-DarkNet](https://gitlab.com/aminehy/yolov3-darknet)

-   Clone and the repository then run using the script `docker_TensorRT_OpenCV_Python.sh`.

    ```bash
    git clone https://gitlab.com/aminehy/yolov3-darknet.git

    cd yolov3-darknet

    chmod +x docker_TensorRT_OpenCV_Python.sh

    ./docker_TensorRT_OpenCV_Python.sh run
    ```

-   Download and unzip the test images folder in `./data/`

    ```bash
    wget http://images.cocodataset.org/zips/test2017.zip
    unzip test2017.zip ../test2017/
    ```

-   Download the weight file `yolov3.weights`

    ```bash
    wget https://pjreddie.com/media/files/yolov3.weights
    ```

-   Then execute YOLOv3 python file:

    ```bash
    python darknet.py
    ```

-   **Results:**

The results should be saved in the folder `./data/results`

<p align="center">
  <img src="https://cdn-images-1.medium.com/max/2000/1*ySnKobAGy8_NFze7U0goYQ.png" alt="pooling"/>
</p>

  `Output: The mean recognition time over 500 images is 0.044 seconds`

## 2) Optimizing and Running YOLOv3 using NVIDIA TensorRT in Python

![](https://cdn-images-1.medium.com/max/2246/1*XM_KOVzJlT1F3sqP1Azfeg.png)

The first step is to import the model, which includes loading it from a saved file on disk and converting it to a TensorRT network from its native framework or format. Our example loads the model in ONNX format from the ONNX model.

> ONNX is a standard for representing deep learning models enabling them to be transferred between frameworks. (Many frameworks such as Caffe2, Chainer, CNTK, PaddlePaddle, PyTorch, and MXNet support the ONNX format).

Next, an optimized TensorRT engine is built based on the input model, target GPU platform, and other configuration parameters specified. The last step is to provide input data to the TensorRT engine to perform inference.

The sample uses the following components in TensorRT to perform the above steps:

-   ONNX parser: takes a trained model in ONNX format as input and populates a network object in TensorRT
-   Builder: takes a network in TensorRT and generates an engine that is optimized for the target platform
-   Engine: takes input data, performs inferences and emits inference output
-   Logger: object associated with the builder and engine to capture errors, warnings, and other information during the build and inference phases

Link to the project: [Amine Hy / YOLOv3-Darknet-ONNX-TensorRT](https://gitlab.com/aminehy/YOLOv3-Darknet-ONNX-TensorRT)

-   Get the project from GitLab and change the working directory

    ```bash
    git clone https://gitlab.com/aminehy/YOLOv3-Darknet-ONNX-TensorRT.git

    cd YOLOv3-Darknet-ONNX-TensorRT/
    ```

-   Convert the model from Darknet to ONNX. This step will create an engine called: `yolov3.onnx`

    ```bash
    python yolov3_to_onnx.py
    ```

-   Convert the model from ONNX to TensorRT. This step will create an engine called: `yolov3.trt` and use for the inference

    ```bash
    python onnx_to_tensorrt.py
    ```

-   For this experiment, we set this parameter: builder.fp16_mode = True

    ```python
    builder.fp16_mode = True
    builder.strict_type_constraints = True
    ```

-   **Results:**

<p align="center">
  <img src="https://cdn-images-1.medium.com/max/2000/1*zcFERKUD21QuPYmK945eiw.png" alt="yolo performance"/>
</p>

    `Output: The mean recognition time over 500 images is 0.018 seconds using the precision fp16.`

**Therefore, using NVIDIA TensorRT is 2.31 x faster than the unoptimized version!.**

## 3) Optimizing and Running YOLOv3 using NVIDIA TensorRT by importing a Caffe model in C++

![](https://cdn-images-1.medium.com/max/2246/1*N1LE_DRYsRdwHWJh6kkE_w.png)

Link to the project: [Amine Hy / YOLOv3-Caffe-TensorRT](https://gitlab.com/aminehy/YOLOv3-Caffe-TensorRT)

-   Get the project and change the working directory

    ```bash
      cd YOLOv3-Caffe-TensorRT/
      ./docker_TensorRT_OpenCV_Python.sh run
    ```

    **Note**: If the folder `/Caffe` does not exist (for any reason), please download the model architecture and weights of YOLOv3 (.prototxt and .caffemodel) from Google Drive and insert them in a folder ‘Caffe’. Two options are possible, the 416 model and the 608 model. These parameters represent the height/width of the image at the input of the YOLOv3 network.

-   Download the caffe model from [Google Drive](https://drive.google.com/drive/folders/18OxNcRrDrCUmoAMgngJlhEglQ1Hqk_NJ).

-   Compile and build the model

    ```bash
    git submodule update — init — recursive

    mkdir build

    cd build && cmake .. && make && make install && cd ..
    ```

-   Edit the configuration file and choose between the settings, YOLO 416 or YOLO 608, located in

    ```bash
    ~/TensorRT-Yolov3/tensorRTWrapper/code/include/YoloConfigs.h
    ```

-   You need also to take a look at the configuration file `configs.h` located in

    ```bash
    ~/TensorRT-Yolov3/include/configs.h
    ```

-   Create the TensorRT engine and run inference on a test image `dog.jpg`

    ```bash
    # for yolov3–416 (don’t forget to edit YoloConfigs.h for YoloKernel)
    ./install/runYolov3 — caffemodel=./caffe/yolov3_416.caffemodel \
    — prototxt=./caffe/yolov3_416.prototxt — input=./dog.jpg \
    — W=416 — H=416 — class=80 — mode=fp16
    ```

-   Once the engine is created, you can pass it as an argument

    ```bash
    ./install/runYolov3 — caffemodel=./caffe/yolov3_416.caffemodel
     — prototxt=./caffe/yolov3_416.prototxt — input=./dog.jpg — W=416 — H=416 — class=80 — enginefile=./engine/yolov3_fp32.engine
    ```

-   **Results**:

      `Output: Time over all layers: 21.245 ms`

**Same observation as the previous experiment, using NVIDIA TensorRT is 2.07 x faster than the non-optimized version!.**

# Conclusion

This article presented the importance of optimizing a pre-trained deep learning model. We illustrated that on an example on object detection application for computer vision, where we obtained a speedup ratio of  > 2 of the inference time.

# References

-   CUDA: <https://developer.nvidia.com/cuda-zone>

-   Docker-CE [https://docs.docker.com/](https://docs.docker.com/v17.09/engine/installation/linux/docker-ce/ubuntu/)

-   NVIDIA TensorRT: <https://ngc.nvidia.com/catalog/containers/nvidia:tensorrt>

-   NVIDIA Container Best Practices: <https://docs.nvidia.com/deeplearning/frameworks/bp-docker/index.html#docker-bp-topic>

-   TensorRT Release 19.05: <https://docs.nvidia.com/deeplearning/sdk/tensorrt-container-release-notes/rel_19-05.html#rel_19-05>

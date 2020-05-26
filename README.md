# Final project of CISC 642

## Introduction

This repo contains drive code of a recently released model, i.e., YOLOv4.

## Rescription

Original repo is referred to https://github.com/AlexeyAB/darknet. The author mainly experiments on MSCOCO. But this repo mainly experiments on VOC dataset. 

## Requirements

1. Window/Linux
2. **CMake >= 3.12**: https://cmake.org/download/
3. **CUDA 10.0**: https://developer.nvidia.com/cuda-toolkit-archive (on Linux do [Post-installation Actions](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#post-installation-actions))
4. **OpenCV >= 2.4**: use your preferred package manager (brew, apt), build from source using [vcpkg](https://github.com/Microsoft/vcpkg) or download from [OpenCV official site](https://opencv.org/releases.html) (on Windows set system variable `OpenCV_DIR` = `C:\opencv\build` - where are the `include` and `x64` folders [image](https://user-images.githubusercontent.com/4096485/53249516-5130f480-36c9-11e9-8238-a6e82e48c6f2.png))
5. **cuDNN >= 7.0 for CUDA 10.0** https://developer.nvidia.com/rdp/cudnn-archive (on **Linux** copy `cudnn.h`,`libcudnn.so`... as desribed here https://docs.nvidia.com/deeplearning/sdk/cudnn-install/index.html#installlinux-tar , on **Windows** copy `cudnn.h`,`cudnn64_7.dll`, `cudnn64_7.lib` as desribed here https://docs.nvidia.com/deeplearning/sdk/cudnn-install/index.html#installwindows )
6. **GPU with CC >= 3.0**: https://en.wikipedia.org/wiki/CUDA#GPUs_supported
7. on Linux **GCC or Clang**, on Windows **MSVC 2015/2017/2019** https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=Community

## Dataset

download VOC2007 + 2012

```
$ sh data/VOC2007.sh
$ sh data/VOC2012.sh
```

And data will placed in data/.

python Then label Train/Test/Val detection datasets

```
$ cd data
$ python voc_label.py
```

## Weights

1. **yolov4-ciou.weights**:https://drive.google.com/open?id=1IsLdGVkgTHl4G0EbZzb4PlIyfyyjXd9E
2. **yolov4-mse.weights**:https://drive.google.com/open?id=1znVte9XIyWjUGbzh1Cif8IWG4jxswn5L
3. **yolov4-giou.weights**:https://drive.google.com/open?id=1TTZ5Q_mJ4DYfJBp_DnoZXSMGbAJ7zIcn
4. **yolov4-diou.weights**:https://drive.google.com/open?id=1XEez2aGAnS60kD4VFzQ7LM8wppAdKpBj
5. (Train-from-scratch)**yolov4.conv.137**: https://drive.google.com/open?id=1N7_Gea1gol6ZaGfB8XBnreaeH3D9vZ6f




## Train
Before you start train or test, make sure you are at the folder`Chris-s-darknet`.

First, place model file in folder you like. Below, I place in `./weights`. By running one of following lines, a loss plot will be saved in current folder with name like `chart_yolov4-ciou.png`:

<img src="https://i.loli.net/2020/05/23/2O8zRXU5q4lJAh3.png" alt="chart_yolov4-ciou" style="zoom: 67%;" />

### MSE loss

```
$ ./build-release/darknet detector train cfg/voc.data cfg/yolov4-mse.cfg weights/yolov4.conv.137 -dont_show -map
```

### CIOU loss

```
$ ./build-release/darknet detector train cfg/voc.data cfg/yolov4-ciou.cfg weights/yolov4.conv.137 -dont_show -map
```

### IOU loss

```
$ ./build-release/darknet detector train cfg/voc.data cfg/yolov4-iou.cfg weights/yolov4.conv.137 -dont_show -map
```



## Test

Here replace `backup2/yolov4-ciou.weights` with model you want to test.

```
$ ./build-release/darknet detector map cfg/voc.data cfg/yolov4-ciou.cfg backup2/yolov4-ciou.weights -dont_show -map
```

### Test with different `nms` method:

If you want to test mAP with different NMS method, change three lines in which `.cfg` you are using:

For example, I want to test on YOLOv4 trained with CIOU loss, so I edit `./cfg/yolov4-ciou.cfg`:

<img src="https://i.loli.net/2020/05/25/IsZDhQP5YbRmzUu.png" alt="image-20200524181824285" style="zoom:50%;" />

Optional NMS are: 

1. greedynms
2. cornersnms
3. diounms

### Test with different `IoU threshold`:

The default `iou_thresh` is set to 0.5. If want to test with different `iou_thresh`, e.g., 0.75, run:

```
$ ./build-release/darknet detector map cfg/voc.data cfg/yolov4-ciou.cfg backup2/yolov4-ciou.weights -dont_show -map -iou_thresh 0.75
```


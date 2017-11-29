# Tensorflow OSX Build

Unfortunately, the Tensorflow team stops releasing binary package for Mac OS with CUDA support since Tensorflow 1.2. This project provides off-the-shelf binary packages.

很不幸，Tensorflow团队自从1.2版本开始停止了发布 Mac OS CUDA版。本项目提供 Mac OS 上编译好、可直接安装的的Tensorflow CUDA版本。


# Releases

| FileName | Tensorflow  | CUDA | CUDNN | Compute Compatibility | Compilation Time |
|:--:|:--:|:--:|:--:|:--:|:--:|
| tensorflow-1.4.0.dev0-cp27-cp27m-macosx\_10\_12\_intel.whl | 1.4.0.dev0 | 8.0 | 6 | 3.0,3.5,5.2,6.1 | 2017-09-15 |
| tensorflow-1.4.0-cp27-cp27m-macosx\_10\_12\_intel.whl | 1.4.0 | 8.0 | 6 | 3.0,3.5,5.2,6.1 | 2017-11-29 |


# Installation

First, ensure your CUDA driver and cudnn is installed properly.

首先，确保CUDA驱动和cudnn正确安装。

Second, uninstall the previous tensorflow installtion by

其次，卸载之前版本的Tensorflow:

```
pip uninstall tensorflow
pip uninstall tensorflow-gpu
```

At last, uncompress and install the binary package from this project:

最后，解压并安装：

```
cat tensorflow-1.4.0.dev0-cp27-cp27m-macosx_10_12_intel.whl.zip.* > tensorflow-1.4.0.dev0-cp27-cp27m-macosx_10_12_intel.whl.zip
unzip tensorflow-1.4.0.dev0-cp27-cp27m-macosx_10_12_intel.whl.zip
pip install tensorflow-tensorflow-1.4.0.dev0-cp27-cp27m-macosx_10_12_intel.whl
```

Enjoy!

开始使用新版Tensorflow吧!


# Source Code

Source code from: [https://github.com/tensorflow/tensorflow](https://github.com/tensorflow/tensorflow)
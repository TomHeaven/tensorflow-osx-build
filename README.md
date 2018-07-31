# Tensorflow OSX Build

Unfortunately, the Tensorflow team stops releasing binary package for Mac OS with CUDA support since Tensorflow 1.2. This project provides off-the-shelf binary packages. ``Both Python 2.7 and 3.6 are supported now!``

很不幸，Tensorflow团队自从1.2版本开始停止了发布 Mac OS CUDA版。本项目提供 Mac OS 上编译好、可直接安装的Tensorflow CUDA版本。``本项目同时支持Python 2.7 和 3.6 了！``

# Attenion
+ 【2018.07.31】Currently, Nvidia provides CUDA 9.2 driver for Mac OS High Sierra (10.13) only. Therefore, do NOT download the CUDA 9.2 versions if you are using Sierra (10.12).
+ 【2018.07.31】目前，英伟达仅在10.13系统是提供CUDA 9.2。所以如果你使用10.12系统，不要下载CUDA 9.2版本安装包。


# Releases

| FileName | Tensorflow  | CUDA | CUDNN | Compute Capability | Compilation Time |
|:--:|:--:|:--:|:--:|:--:|:--:|
| tensorflow-1.8.0-cp36-cp36m-macosx\_10\_12\_x86_64.whl | 1.8.0 | 9.2 | 7.1 | 3.0,3.5,5.2,6.1 | 2018-07-30 |
| tensorflow-1.8.0-cp27-cp27m-macosx\_10\_12\_intel.whl | 1.8.0 | 9.2 | 7.1 | 3.0,3.5,5.2,6.1 | 2018-07-30 |
| tensorflow-1.6.0-cp36-cp36m-macosx\_10\_12\_intel.whl | 1.6.0 | 9.0 | 7 | 3.0,3.5,5.2,6.1 | 2018-04-23 |
| tensorflow-1.6.0-cp27-cp27m-macosx\_10\_12\_intel.whl | 1.6.0 | 9.0 | 7 | 3.0,3.5,5.2,6.1 | 2018-04-23 |
| tensorflow-1.4.0-cp27-cp27m-macosx\_10\_12\_intel.whl | 1.4.0 | 8.0 | 6 | 3.0,3.5,5.2,6.1 | 2017-11-29 |
| tensorflow-1.4.0.dev0-cp27-cp27m-macosx\_10\_12\_intel.whl | 1.4.0.dev0 | 8.0 | 6 | 3.0,3.5,5.2,6.1 | 2017-09-15 |

You can find older releases in the [releases page](https://github.com/TomHeaven/tensorflow-osx-build/releases).

你可以在[Releases页面](https://github.com/TomHeaven/tensorflow-osx-build/releases)找到以前发布的版本。


# Installation for Python 2.7

First, ensure your CUDA driver and cudnn is installed properly, and copy dependencies in folder `usr_local_lib` to path `/usr/local/lib`.

首先，确保CUDA驱动和cudnn正确安装,并且将文件夹`usr_local_lib`中的依赖项复制到路径`/usr/local/lib`。

```
sudo mkdir /usr/local
sudo mkdir /usr/local/lib
sudo cp usr_local_lib/* /usr/local/lib/
```

Second, uninstall the previous tensorflow installtion by

其次，卸载之前版本的Tensorflow:

```
pip uninstall tensorflow
pip uninstall tensorflow-gpu
```

At last, uncompress and install the binary package from this project:

最后，解压并安装：

```
cat tensorflow* > tensorflow.whl.zip
unzip tensorflow.whl.zip
pip install tensorflow*.whl
```

# Installation for Python 3.6

Install Python 3.6 from Homebrew first, and then simply follow the guide for Python 2.7 and replace `pip` command with `pip3` and `python` with `python3`.

首先从Homebrew安装Python 3.6，然后按照Python 2.7的安装步骤执行，注意将`pip`替换为`pip3`，并用`python3`启动`python`。



Enjoy!

开始使用新版Tensorflow吧!


# Source Code

Source code from: [https://github.com/tensorflow/tensorflow](https://github.com/tensorflow/tensorflow)

# Related Links

If you need Pytorch builds for osx, go to this page: [https://github.com/TomHeaven/pytorch-osx-build](https://github.com/TomHeaven/pytorch-osx-build)

如果你需要Pytorch包，请看这个页面：[https://github.com/TomHeaven/pytorch-osx-build](https://github.com/TomHeaven/pytorch-osx-build)
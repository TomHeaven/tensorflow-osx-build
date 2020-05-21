# Tensorflow OSX Build

Unfortunately, the Tensorflow team stops releasing binary package for Mac OS with CUDA support since Tensorflow 1.2. This project provides off-the-shelf binary packages. ``Both Python 2.7 and 3.7 are supported now!``

很不幸，Tensorflow团队自从1.2版本开始停止了发布 Mac OS CUDA版。本项目提供 Mac OS 上编译好、可直接安装的Tensorflow CUDA版本。``本项目同时支持Python 2.7 和 3.7 了！``



# Releases

You can find releases in the [releases page](https://github.com/TomHeaven/tensorflow-osx-build/releases).

你可以在[Releases页面](https://github.com/TomHeaven/tensorflow-osx-build/releases)找到以前发布的版本。

# My Fork of Tensorflow

Except for making patches for release verions of TF, I fork TF sources at [https://github.com/TomHeaven/tensorflow](https://github.com/TomHeaven/tensorflow) and keep fixing build issues of TF on macOS with CUDA. The corresponding PR is at: [https://github.com/tensorflow/tensorflow/pull/39297](https://github.com/tensorflow/tensorflow/pull/39297). You can use the PR to make your own builds.


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
pip uninstall tensorflow-gpu # for early version with offical support
```

At last, download binary packages from [Releases](https://github.com/TomHeaven/tensorflow-osx-build/releases) page and install

最后，从[Releases页面](https://github.com/TomHeaven/tensorflow-osx-build/releases)下载并安装：

```

pip install tensorflow*.whl
```

# Installation for Python 3.7

Install Python 3.7 from Homebrew first, and then simply follow the guide for Python 2.7 and replace `pip` command with `pip3` and `python` with `python3`.

首先从Homebrew安装Python 3.7，然后按照Python 2.7的安装步骤执行，注意将`pip`替换为`pip3`，并用`python3`启动`python`。



Enjoy!

开始使用新版Tensorflow吧!

# Build Tutorial
If you want to build your own wheel packages, refer to the following tutorials:

+ [v1.10](https://github.com/TomHeaven/tensorflow-osx-build/blob/master/build_tutorial_1.10.md)
+ [v2.0.0](https://github.com/TomHeaven/tensorflow-osx-build/blob/master/build_tutorial_2.0.0.md) This tutorial also works for v1.15.0, just use source patch v1.15.0 instead of v2.0.0.



# Related Links

If you need Pytorch builds for osx, go to this page: [https://github.com/TomHeaven/pytorch-osx-build](https://github.com/TomHeaven/pytorch-osx-build)

If you need MxNet builds for osx, go to this page: [https://github.com/TomHeaven/mxnet_osx_build](https://github.com/TomHeaven/mxnet_osx_build)

如果你需要Pytorch包，请看这个页面：[https://github.com/TomHeaven/pytorch-osx-build](https://github.com/TomHeaven/pytorch-osx-build)

如果你需要MxNet包，请看这个页面：[https://github.com/TomHeaven/mxnet_osx_build](https://github.com/TomHeaven/mxnet_osx_build)
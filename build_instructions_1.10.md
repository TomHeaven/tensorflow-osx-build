# Tensorflow OSX Build Insructions (1.10)

By `Tom Heaven` @ 2018.08.25

Project page: [https://github.com/TomHeaven/tensorflow-osx-build](https://github.com/TomHeaven/tensorflow-osx-build)

---

Note patches for every release of Tensorflow is a bit different! So the instructions work for v1.10.0 only.

First make sure your XCode 8, CUDA 9.0 SDK and CUDNN 7 are properly installed, and Internet connection is also required. You need to install Python3 using `Homebrew` if you need to compile for Python3.

If running on Mac OS High Sierra(10.13), you need to keep both XCode 8 (CUDA 9 SDK only supports this version) and XCode 9 (Homebrew requires this version) and switch between them by renaming.

I've found that build against CUDA 9.2 does not work properly, which keeps reporting `CUDA OUT OF MEMORY ERROR` and my test program is easily killed by operating system. If you have interests, feel free to try.


## For Python 2.7 and 3.6

The following instructions will help you build your own wheel files for Python 2.7 and 3.6 using CUDA 9.0.


+ download and configure `nccl`. Start a terminal and run

```shell
wget -q https://developer.download.nvidia.com/compute/redist/nccl/v2.1/nccl_2.2.13-1+cuda9.0_x86_64.txz
cd nccl_2.2.13-1+cuda9.1_x86_64
mkdir /usr/local/nccl
cp -r * /usr/local/nccl
# create symbolic links or TF will not find NCCL when linking
cd /usr/local/nccl/lib
ln -s -f libnccl.so.2.2.13 /usr/local/nccl/lib/libnccl.2.dylib 
ln -s -f libnccl.so.2.2.13 /usr/local/nccl/lib/libnccl.2.2.13.dylib 
# export the following variables so TF can find nccl
export TF_NCCL_VERSION=2.2.13
export NCCL_INSTALL_PATH=/usr/local/nccl
```
Note `nccl` does not work on Mac OS X. However, we need the files so that the configuration of Tensorflow works without errors. As we export temporary variables, the following commands should be executed in the same terminal.


+ download Tensorflow sources and switch to `v1.10.0`

```shell
git clone https://github.com/tensorflow/tensorflow
cd tensorflow
git checkout v1.10.0
```
+ patch sources using the following shell commands:

```shell
# new patches
sed "s/__align__(sizeof(T))/__align__(sizeof(T) > 16 ? sizeof(T) : 16)/" "tensorflow/core/kernels/split_lib_gpu.cu.cc" > tmp.h.cc
cp -f tmp.h.cc "tensorflow/core/kernels/split_lib_gpu.cu.cc"
rm -f tmp.h.cc
## 2
sed "s/__align__(sizeof(T))/__align__(sizeof(T) > 16 ? sizeof(T) : 16)/" "tensorflow/core/kernels/depthwise_conv_op_gpu.cu.cc" > tmp.h.cc
cp -f tmp.h.cc "tensorflow/core/kernels/depthwise_conv_op_gpu.cu.cc"
rm -f tmp.h.cc
## 3
sed "s/__align__(sizeof(T))/__align__(sizeof(T) > 16 ? sizeof(T) : 16)/" "tensorflow/core/kernels/concat_lib_gpu_impl.cu.cc" > tmp.h.cc
cp -f tmp.h.cc "tensorflow/core/kernels/concat_lib_gpu_impl.cu.cc"
rm -f tmp.h.cc

## 4 patch for nccl configuration
sed "s/libnccl.so.%s/libnccl.%s.dylib/" "third_party/nccl/nccl_configure.bzl" > tmp.h.cc
cp -f tmp.h.cc "third_party/nccl/nccl_configure.bzl"
rm -f tmp.h.cc
```
+ run `./configure`, note that:  
  - using `/usr/local/bin/python3` if you want a build for python3.
  - select the correct site packages path.
  - Set correct CUDA compute capability values.
  - Use `/usr/bin/gcc` as the default compiler.
  
Here is an example for Python 2.7:

```
MacBook-Pro:tensorflow-master tomheaven$ ./configure 
You have bazel 0.5.1-homebrew installed.
Please specify the location of python. [Default is /usr/bin/python]: 


Found possible Python library paths:
  /Library/Python/2.7/site-packages
  /Users/tomheaven/Documents/caffe-master/python
  /Users/tomheaven/Documents/Current/demosaicnet-master/demosaicnet
  /Users/tomheaven/Documents/facenet/build/lib/src
Please input the desired Python library path to use.  Default is [/Library/Python/2.7/site-packages]

Do you wish to build TensorFlow with Google Cloud Platform support? [y/N]: 
No Google Cloud Platform support will be enabled for TensorFlow.

Do you wish to build TensorFlow with Hadoop File System support? [y/N]: 
No Hadoop File System support will be enabled for TensorFlow.

Do you wish to build TensorFlow with XLA JIT support? [y/N]: 
No XLA JIT support will be enabled for TensorFlow.

Do you wish to build TensorFlow with GDR support? [y/N]: 
No GDR support will be enabled for TensorFlow.

Do you wish to build TensorFlow with VERBS support? [y/N]: 
No VERBS support will be enabled for TensorFlow.

Do you wish to build TensorFlow with OpenCL support? [y/N]: 
No OpenCL support will be enabled for TensorFlow.

Do you wish to build TensorFlow with CUDA support? [y/N]: y
CUDA support will be enabled for TensorFlow.

Please specify the CUDA SDK version you want to use, e.g. 9.0. [Leave empty to default to CUDA 9.0]: 


Please specify the location where CUDA 9.0 toolkit is installed. Refer to README.md for more details. [Default is /usr/local/cuda]: 


Please specify the cuDNN version you want to use. [Leave empty to default to cuDNN 7.0]: 


Please specify the location where cuDNN 7 library is installed. Refer to README.md for more details. [Default is /usr/local/cuda]:


Please specify a list of comma-separated Cuda compute capabilities you want to build with.
You can find the compute capability of your device at: https://developer.nvidia.com/cuda-gpus.
Please note that each additional compute capability significantly increases your build time and binary size. [Default is: 3.5,5.2] 3.0,3.5,5.2,6.1


Do you want to use clang as CUDA compiler? [y/N]: n
nvcc will be used as CUDA compiler.

Please specify which gcc should be used by nvcc as the host compiler. [Default is /usr/local/bin/gcc]: /usr/bin/gcc

Do you wish to build TensorFlow with MPI support? [y/N]: n
No MPI support will be enabled for TensorFlow.
```
+ start building

```
bazel build --config=opt //tensorflow/tools/pip_package:build_pip_package
```
+ generate a wheel package
```
bazel-bin/tensorflow/tools/pip_package/build_pip_package ./
```

## For Python 3.7
Python 3.7 is newly released. There are a few major changes affecting Tensorflow v1.10.0:

+ PyUnicode_AsXXXAndSize() function returns `const char*` rather than `char*`. Refer to this post: [https://github.com/protocolbuffers/protobuf/pull/4862/files](https://github.com/protocolbuffers/protobuf/pull/4862/files) for patches to protobuf. Similar patches are required for the following files:

 - `tensorflow/python/lib/core/ndarray_tensor.cc:157:13`
 - `tensorflow/python/lib/core/py_func.cc:355:16`
 - `tensorflow/python/eager/pywrap_tfe_src.cc:219:11`

+ `async` is a reserved word now. Replace function parameter `async` with `t_async` (or any other valid parameter name) in `/tensorflow/c/eager/c_api`. There are four places requiring the patch.

After applying the additional patches, Tensorflow 1.10.0 should be built on Python 3.7.



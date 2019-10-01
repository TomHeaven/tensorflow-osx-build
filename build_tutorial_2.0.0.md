# Tensorflow OSX Build Tutorial (v2.0.0)

By `Tom Heaven` @ 2019.10.01

Project page: [https://github.com/TomHeaven/tensorflow-osx-build](https://github.com/TomHeaven/tensorflow-osx-build)

---

Note patches for every release of Tensorflow is different! So the instructions work for v2.0.0 only.

First make sure your XCode 9.4.1, CUDA 10.0 SDK and CUDNN 7.4 are properly installed, and Internet connection is also required. Install Python3 using `Homebrew` if you need to compile for Python3.

If running on Mac OS High Sierra(10.13), you need to keep both XCode 8 (CUDA 9 SDK only supports this version) and XCode 9 (Homebrew requires this version) and switch between them by renaming.


## For Python 3.7
The following instructions will help you build your own wheel files for Python 3.7 with CUDA 10.0.

+ download Tensorflow sources and switch to `v2.0.0`

```shell
git clone https://github.com/tensorflow/tensorflow
cd tensorflow
git checkout v2.0.0
```
+ download and use source patches in this repo to patch sources:

```shell
git apply v2.0.0_macos.patch

```
+ run `./configure`, note that:  
  - using `/usr/local/bin/python3` if you want a build for python3.
  - select the correct site packages path.
  - Set correct CUDA compute capability values.
  - Use `/usr/bin/gcc` as the default compiler.
  
Here is an example for Python 3.7:

```
iMac18:tensorflow tomheaven$ ./configure
WARNING: Running Bazel server needs to be killed, because the startup options are different.
WARNING: --batch mode is deprecated. Please instead explicitly shut down your Bazel server using the command "bazel shutdown".
You have bazel 0.24.1 installed.
Please specify the location of python. [Default is /usr/bin/python]: /usr/local/bin/python3


Found possible Python library paths:
  /usr/local/Cellar/python/3.7.0/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages
Please input the desired Python library path to use.  Default is [/usr/local/Cellar/python/3.7.0/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages]

Do you wish to build TensorFlow with XLA JIT support? [Y/n]: n
No XLA JIT support will be enabled for TensorFlow.

Do you wish to build TensorFlow with OpenCL SYCL support? [y/N]: 
No OpenCL SYCL support will be enabled for TensorFlow.

Do you wish to build TensorFlow with ROCm support? [y/N]: 
No ROCm support will be enabled for TensorFlow.

Do you wish to build TensorFlow with CUDA support? [y/N]: y
CUDA support will be enabled for TensorFlow.

Found CUDA 10.0 in:
    /usr/local/cuda/lib64
    /usr/local/cuda/include
Found cuDNN 7 in:
    /usr/local/cuda/lib64
    /usr/local/cuda/include


Please specify a list of comma-separated CUDA compute capabilities you want to build with.
You can find the compute capability of your device at: https://developer.nvidia.com/cuda-gpus.
Please note that each additional compute capability significantly increases your build time and binary size, and that TensorFlow only supports compute capabilities >= 3.5 [Default is: 3.5,7.0]: 3.0,3.5,5.0,5.2,6.1,7.0


WARNING: XLA does not support CUDA compute capabilities lower than 3.5. Disable XLA when running on older GPUs.
Do you want to use clang as CUDA compiler? [y/N]: 
nvcc will be used as CUDA compiler.

Please specify which gcc should be used by nvcc as the host compiler. [Default is /usr/local/bin/gcc]: /usr/bin/gcc


Do you wish to build TensorFlow with MPI support? [y/N]: 
No MPI support will be enabled for TensorFlow.

Please specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is -march=native -Wno-sign-compare]: 


Would you like to interactively configure ./WORKSPACE for Android builds? [y/N]: 
Not configuring the WORKSPACE for Android builds.

Do you wish to build TensorFlow with iOS support? [y/N]: 
No iOS support will be enabled for TensorFlow.

Preconfigured Bazel build configs. You can use any of the below by adding "--config=<>" to your build command. See .bazelrc for more details.
	--config=mkl         	# Build with MKL support.
	--config=monolithic  	# Config for mostly static monolithic build.
	--config=gdr         	# Build with GDR support.
	--config=verbs       	# Build with libverbs support.
	--config=ngraph      	# Build with Intel nGraph support.
	--config=numa        	# Build with NUMA support.
	--config=dynamic_kernels	# (Experimental) Build kernels into separate shared objects.
	--config=v2          	# Build TensorFlow 2.x instead of 1.x.
Preconfigured Bazel build configs to DISABLE default on features:
	--config=noaws       	# Disable AWS S3 filesystem support.
	--config=nogcp       	# Disable GCP support.
	--config=nohdfs      	# Disable HDFS support.
	--config=noignite    	# Disable Apache Ignite support.
	--config=nokafka     	# Disable Apache Kafka support.
	--config=nonccl      	# Disable NVIDIA NCCL support.
Configuration finished
```
+ start building

```
bazel build --config=opt --config=nonccl //tensorflow/tools/pip_package:build_pip_package
```
Note `NCCL` only works on Linux now, which does not work on Mac OS X or Windows. We need to disable it by passing `--config=nonccl`, or you will meet NCCL related errors.

+ fix a source error in external sources. Previous building process will end with an error:
+ 
```
external/com_google_absl/absl/container/internal/compressed_tuple.h:170:53: error: use 'template' keyword to treat 'Storage' as a dependent template name
return (std::move(*this).internal_compressed_tuple::Storage< CompressedTuple, I> ::get()); 
```

Solution: Edit `bazel-tensorflow/external/com_google_absl/absl/container/internal/compressed_tuple.h:168-178` and comment out two problematic functions:

```cpp

/*template <int I>
  ElemT<I>&& get() && {
    return std::move(*this).internal_compressed_tuple::template Storage<CompressedTuple, I>::get();
  }
  template <int I>
  constexpr const ElemT<I>&& get() const&& {
    return absl::move(*this).internal_compressed_tuple::template Storage<CompressedTuple, I>::get();
  }*/

```
Then, build again using the same previous commands. This time, the building process should be successful.

+ generate a wheel package

```
bazel-bin/tensorflow/tools/pip_package/build_pip_package ./
```

## For Python 2.7

For Python 2.7, we need an additional patch for external source file. Edit file `bazel-tensorflow/external/cython/Cython/Compiler/Nodes.py` and add the following lines at top:

```python
import sys
if sys.version < '3':
    reload(sys)
    sys.setdefaultencoding('utf-8')
```
Note external source patch can be applied only after the first build failure because external sources are downloaded at the beginning of the building process.



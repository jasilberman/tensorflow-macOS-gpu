<div align="center">
  <img src="https://www.tensorflow.org/images/tf_logo_transp.png"><br><br>
</div>

-----------------

# Tensorflow

```
Note: As of version 1.2, TensorFlow no longer provides GPU support on macOS.
```

This repo try to build tensorflow-gpu on macOS 

Thanks for https://gist.github.com/smitshilu/53cf9ff0fd6cdb64cca69a7e2827ed0f

### Download

### System information
* OS - High Sierra 10.13
* Tensorflow - 1.5
* Xcode command line tools - 8.3.2 
* Cmake - 3.10.1
* Bazel - 0.9.0-homebrew
* CUDA - 9.1
* cuDNN - 7


### Requirements
* sudo pip install six numpy wheel
* brew install coreutils

### Steps:
 * Disable SIP (in recovery mode enter command: csrutil disable)
 * ./configure   (Find CUDA compute value from https://developer.nvidia.com/cuda-gpus)
     ```
     Smit-Shilu:tensorflow-build smitshilu$ ./configure
     You have bazel 0.7.0-homebrew installed.
     Please specify the location of python. [Default is /Users/smitshilu/anaconda3/bin/python]:

     Found possible Python library paths:
       /Users/smitshilu/anaconda3/lib/python3.6/site-packages
     Please input the desired Python library path to use.  Default is [/Users/smitshilu/anaconda3/lib/python3.6/site-packages]

     Do you wish to build TensorFlow with Google Cloud Platform support? [Y/n]: n
     No Google Cloud Platform support will be enabled for TensorFlow.

     Do you wish to build TensorFlow with Hadoop File System support? [Y/n]: n
     No Hadoop File System support will be enabled for TensorFlow.

     Do you wish to build TensorFlow with Amazon S3 File System support? [Y/n]: n
     No Amazon S3 File System support will be enabled for TensorFlow.

     Do you wish to build TensorFlow with XLA JIT support? [y/N]: n
     No XLA JIT support will be enabled for TensorFlow.

     Do you wish to build TensorFlow with GDR support? [y/N]: n
     No GDR support will be enabled for TensorFlow.

     Do you wish to build TensorFlow with VERBS support? [y/N]: n
     No VERBS support will be enabled for TensorFlow.

     Do you wish to build TensorFlow with OpenCL support? [y/N]: n
     No OpenCL support will be enabled for TensorFlow.

     Do you wish to build TensorFlow with CUDA support? [y/N]: y
     CUDA support will be enabled for TensorFlow.

     Please specify the CUDA SDK version you want to use, e.g. 7.0. [Leave empty to default to CUDA 8.0]: 9.0

     Please specify the location where CUDA 9.0 toolkit is installed. Refer to README.md for more details. [Default is /usr/local/cuda]:

     Please specify the cuDNN version you want to use. [Leave empty to default to cuDNN 6.0]: 7

     Please specify the location where cuDNN 7 library is installed. Refer to README.md for more details. [Default is /usr/local/cuda]:

     Please specify a list of comma-separated Cuda compute capabilities you want to build with.
     You can find the compute capability of your device at: https://developer.nvidia.com/cuda-gpus.
     Please note that each additional compute capability significantly increases your build time and binary size. [Default is: 3.5,5.2]6.1

     Do you want to use clang as CUDA compiler? [y/N]:
     nvcc will be used as CUDA compiler.

     Please specify which gcc should be used by nvcc as the host compiler. [Default is /usr/bin/gcc]:

     Do you wish to build TensorFlow with MPI support? [y/N]:
     No MPI support will be enabled for TensorFlow.

     Please specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is -march=native]:

     Add "--config=mkl" to your bazel command to build with MKL support.
     Please note that MKL on MacOS or windows is still not supported.
     If you would like to use a local MKL instead of downloading, please set the environment variable "TF_MKL_ROOT" every time before build.
     Configuration finished
     ```
 * Add following paths:
   * export CUDA_HOME=/usr/local/cuda
   * export DYLD_LIBRARY_PATH=/Users/__USERNAME__/lib:/usr/local/cuda/lib:/usr/local/cuda/extras/CUPTI/lib (Replace USERNAME with your machine username)
   * export LD_LIBRARY_PATH=$DYLD_LIBRARY_PATH
   * export PATH=$DYLD_LIBRARY_PATH:$PATH
 * Start build
     ```
     bazel build --config=cuda --config=opt --action_env PATH --action_env LD_LIBRARY_PATH --action_env DYLD_LIBRARY_PATH //tensorflow/tools/pip_package:build_pip_package
     ```
 * Generate a wheel for installation 
     ```
     bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
     ```
 * Install tensorflow wheel 
     ```
     sudo pip install /tmp/tensorflow_pkg/tensorflow-1.4.0rc1-cp36-cp36m-macosx_10_7_x86_64.whl (File name depends on tensorflow version and python version)
     ```
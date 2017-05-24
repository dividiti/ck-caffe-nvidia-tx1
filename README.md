# [cknowledge.org/ai](http://cknowledge.org): Crowdsourcing benchmarking and optimisation of AI

## [PUBLIC] Benchmarking Caffe and TensorRT on NVIDIA Jetson TX1

**NB:** The Caffe experimental results are released with approval from General Motors. The TensorRT 1.0 EA experimental results are released with approval from NVIDIA.

The Jupyter notebook ([view on github.com](https://github.com/dividiti/ck-caffe-nvidia-tx1/blob/master/script/caffe-tensorrt/ck-caffe-nvidia-tx1-with-tensorrt.20170429.ipynb); [view on nbviewer.jupyter.org](https://nbviewer.jupyter.org/github/dividiti/ck-caffe-nvidia-tx1/blob/master/script/caffe-tensorrt/ck-caffe-nvidia-tx1-with-tensorrt.20170429.ipynb)) in this Collective Knowledge repository analyses the performance (execution time, memory consumption):

- on **[dividiti](http://dividiti.com)**'s Jetson TX1 board ([official page](http://www.nvidia.com/object/jetson-tx1-dev-kit.html), [Phoronix review](http://www.phoronix.com/scan.php?page=article&item=nvidia-jtx1-perf)):
  - CPU:
    - ARM&reg; Cortex&reg;-A57 architecture ("big");
    - 4 cores;
    - Max clock 1743 MHz;
  - GPU:
    - Maxwell&trade; architecture;
    - 256 CUDA cores;
    - Max clock 998 MHz;
  - RAM:
    - LPDDR4;
    - 4 GB (shared between the CPU and the GPU);
    - Max bandwidth 25.6 GB/s;
  - [Linux for Tegra 24.2.1](http://developer.download.nvidia.com/embedded/L4T/r24_Release_v2.1/Docs/Tegra_Linux_Driver_Package_Release_Notes_R24.2.1.pdf);
  - [JetPack 2.3.1](http://docs.nvidia.com/jetpack-l4t/index.html#developertools/desktop/jetpack/l4t/2.3.1);
  - CUDA Toolkit 8.0.33.
```
$ uname -a
Linux tegra-ubuntu 3.10.96-tegra #1 SMP PREEMPT Wed Nov 9 19:42:57 PST 2016 aarch64 aarch64 aarch64 GNU/Linux
$ cat /etc/lsb-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=16.04
DISTRIB_CODENAME=xenial
DISTRIB_DESCRIPTION="Ubuntu 16.04.1 LTS"
```

- using 6 Caffe **libraries**:
  - [`tag`] **Branch** (**revision hash, date**): **math libraries**.
  - [`cpu`] Master ([24d2f67](https://github.com/BVLC/caffe/commit/24d2f67173db3344141dce24b1008efffbfe1c7d), 28/Nov/2016): with [OpenBLAS 0.2.19](https://github.com/xianyi/OpenBLAS/releases/tag/v0.2.19);
  - [`nvidia-cuda`] NVIDIA 0.15 ([1024d34](https://github.com/NVIDIA/caffe/commit/1024d34d93cd34a9013d6fac4e56e45162073d38), 17/Nov/2016): with [cuBLAS](https://developer.nvidia.com/cublas) (part of CUDA Toolkit 8.0.33);
  - [`nvidia-cudnn`] NVIDIA 0.15 ([1024d34](https://github.com/NVIDIA/caffe/commit/1024d34d93cd34a9013d6fac4e56e45162073d38), 17/Nov/2016): with [cuDNN 5.1](https://developer.nvidia.com/cudnn);
  - [`nvidia-fp16-cuda`] NVIDIA experimental/fp16 ([fca1cf4](https://github.com/NVIDIA/caffe/commit/fca1cf475d1d0a6d355f8b9877abcc4e13951c9c), 11/Jul/2016): with [cuBLAS](https://developer.nvidia.com/cublas) (part of CUDA Toolkit 8.0.33);
  - [`nvidia-fp16-cudnn`] NVIDIA experimental/fp16 ([fca1cf4](https://github.com/NVIDIA/caffe/commit/fca1cf475d1d0a6d355f8b9877abcc4e13951c9c), 11/Jul/2016): with [cuDNN 5.1](https://developer.nvidia.com/cudnn);
  - [`libdnn-cuda`] OpenCL ([b735c2d](https://github.com/BVLC/caffe/commit/b735c2dd4103ac963332b400168507dd7cefd204), 23/Nov/2016): with [libDNN](https://github.com/BVLC/caffe/issues/4155) and [cuBLAS](https://developer.nvidia.com/cublas) (part of CUDA Toolkit 8.0.33) for fully connected layers;

  **NB:** libDNN is not yet tuned for TX1 - it uses parameters that are optimal for GTX 1080.

- using 2 configurations of the [NVIDIA TensorRT 1.0.0 EA engine](https://developer.nvidia.com/tensorrt):
  - [`tensorrt-fp16`] NVIDIA TensorRT 1.0.0 EA with fp16 enabled;
  - [`tensorrt-fp32`] NVIDIA TensorRT 1.0.0 EA with fp16 disabled;

  **NB:** This EA ("early access") version is used in accordance with its special licensing terms: the results are released with explicit written approval from NVIDIA. The results may not be representative of the GA ("general availability") version.

- using 4 CNN **models**:
  - [GoogleNet](https://github.com/BVLC/caffe/tree/master/models/bvlc_googlenet);
  - [AlexNet](https://github.com/BVLC/caffe/tree/master/models/bvlc_alexnet);
  - [SqueezeNet 1.0](https://github.com/DeepScale/SqueezeNet/tree/master/SqueezeNet_v1.0);
  - [SqueezeNet 1.1](https://github.com/DeepScale/SqueezeNet/tree/master/SqueezeNet_v1.1);

- with the **batch size** varying from 2 to 16 with step 2.

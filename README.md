# Rodinia Benchmarks for DPC++
Rodinia benchmarks for CUDA translated to DPC++ using the [Intel DPC++ Compatibility Tool](https://www.intel.com/content/www/us/en/developer/tools/oneapi/dpc-compatibility-tool.html#gs.g3bkj1). These benchmarks can run in CPU, and NVIDIA/Intel GPUs.

## Requirements
Before to run the benchmarks you have to install some dependencies.

* [CUDA Toolkit 11.4](https://developer.nvidia.com/cuda-11-4-0-download-archive) &#8594; Mandatory to run in NVIDIA GPUs.
* [Intel LLVM open source compiler](https://github.com/intel/llvm/blob/sycl/sycl/doc/GetStartedGuide.md) &#8594; It will run the code on all the devices.
* [Intel oneAPI Base Toolkit](https://www.intel.com/content/www/us/en/developer/tools/oneapi/overview.html). The commercial "dpcpp" compiler cohabits with the previous compiler. However, it does not support NVIDIA GPUs.
* In order to run the "Hybridsort" test, you have to install some OpenGL libraries:
    ```
    sudo apt-get install freeglut3 freeglut3-dev
    sudo apt-get install binutils-goldc
    sudo apt-get install libglew1.5
    ```

## Project Configuration
Once you installed all the requirements, you have to edit the file "common/make.config", changing the value of the following variables:

* CUDA_DIR &#8594; set the path where you have installed the CUDA Toolkit (e.g. /usr/local/cuda)
* LLVM_DIR &#8594; set the location where you have installed the LLVM compiler (e.g. ~/sycl_workspace/llvm/build)
* ONEAPI_DIR &#8594; set the location where you have installed the oneAPI Base Toolkit (e.g. /opt/intel/oneapi)

## Compilation
At this point, there are two Makefiles to build the benchmarks, one of them for CUDA benchmarks and another for DPC++ benchmarks.

### CUDA Benchmark Compilation
Move to cuda folder and invoke the make command with the following arguments:

* time=<0,1> &#8594; Prints the time consumption of the device.

Example:
```
cd cuda
make time=1
```

### DPC++ Benchmark Compilation
Move to dpcpp folder and invoke the make command with the following arguments:

* time=<0,1> &#8594; Prints the time consumption of the device.
* DPCPP_ENV=<clang,oneapi> &#8594; The "clang" option uses the LLVM compiler, while the "oneapi" uses the oneAPI compiler.
* DEVICE=<CPU,INTEL_GPU,NVIDIA_GPU> &#8594; Selects the device where the code runs. The "NVIDIA_GPU" option just works selecting the variable "DPCPP_ENV=clang".

The following example compiles the benchmarks using the LLVM compiler, selects the NVIDIA GPU, and choose to show the GPU time consumption:
```
cd dpcpp
make DPCPP_ENV=clang DEVICE=NVIDIA_GPU time=1
```

## How to run it?
You can run them one by one, or use the scripts we provide ("time_cuda.sh", "time_dpcpp.sh"), which save the kernel time in a "timing" folder. For that, you had to compile them with the "time=1" argument.

## Known Issues
### Benchmarks not working in DPC++
The following benchmarks does not work in DPC++:

* Hybridsort
* Kmeans
* Leukocyte
* Mummergpu

## Publications
* Germán Castaño, Youssef Faqir-Rhazoui, Carlos García and Manuel Prieto-Matías (2022). "Evaluation of Intel’s DPC++ Compatibility Tool in Heterogeneous Computing." Journal of Parallel and Distributed Computing, volume 165, pages 120-129.
    * [Free available here](https://doi.org/10.1016/j.jpdc.2022.03.017).

## Acknowledgements
This work has been supported by the EU (FEDER) and the Spanish MINECO and CM under grants S2018/TCS-4423 and RTI2018-093684-B-I00.

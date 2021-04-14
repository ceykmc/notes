# Window编译ONNXRUNTIME

## 基础版本

1. 克隆onnxruntime仓库

   ```shell
   git clone --recursive https://github.com/Microsoft/onnxruntime
   cd onnxruntime/cmake
   mkdir build  # 进行build的目录
   mkdir install # 安装目录
   cd build
   ```

2. 下载protoc

   protoc需要和cmake/external/protobuf中的版本保持一致，可以进行下述操作：

   ```shell
   cd cmake/external/protobuf
   git checkout -b v3.11.3 v3.11.3
   ```

   下载对应版本的protoc：

   https://github.com/protocolbuffers/protobuf/releases/download/v3.11.3/protoc-3.11.3-win64.zip

   解压缩到某个目录

3. 使用cmake和nmake进行编译

   cmake版本需要大于3.13.0，建议官网下载最新的版本。

   **以下命令需要在 x64 Native Tools Command Prompt for VS 2019 中进行**
   
   ```shell
   cd build
   cmake -Donnxruntime_BUILD_SHARED_LIB=ON -DONNX_CUSTOM_PROTOC_EXECUTABLE=/path/to/protoc -DCMAKE_INSTALL_PREFIX=../install -Donnxruntime_USE_AVX=ON -Donnxruntime_USE_AVX2=ON -Donnxruntime_USE_AVX512=ON -DCMAKE_BUILD_TYPE=Release -G"NMake Makefiles" ..
   nmake
   nmake install
   ```
   
   注意上面命令中，protoc的路径需要和实际保持一致。CMAKE_INSTALL_PREFIX指定了安装的路径，可以根据需要，配置到指定的目录。

## OpenVINO Provider

1. 安装OpenVINO

   [OpenVINO下载](!https://software.intel.com/content/www/us/en/develop/tools/openvino-toolkit/download.html#operatingsystem=Windows&#distributions=Web%20&%20Local%20(recommended)&#options=Local)

2. 使用cmake和nmake进行编译

   启动 x64 Native Tools Command Prompt for VS 2019

   切换进 OpenVINO bin 目录，执行 setupvars.bat

   ```shell
   cd E:\workspace\cpp_libs\OpenVINO_202102\openvino_2021\bin
   setupvars.bat
   cd /path/to/onnxruntime/cmake
   cd build
   cmake -Donnxruntime_BUILD_SHARED_LIB=ON -DONNX_CUSTOM_PROTOC_EXECUTABLE=/path/to/protoc -DCMAKE_INSTALL_PREFIX=../install -Donnxruntime_USE_AVX=ON -Donnxruntime_USE_AVX2=ON -Donnxruntime_USE_AVX512=ON -Donnxruntime_USE_OPENVINO=ON -Donnxruntime_USE_OPENVINO_CPU_FP32=ON -Donnxruntime_USE_OPENVINO_DEVICE=CPU_FP32 -Donnxruntime_USE_OPENVINO_BINARY=ON -DCMAKE_BUILD_TYPE=Release -G"NMake Makefiles" ..
   nmake
   nmake install
   ```

   ## CUDA Provider

   1. 安装CUDA和cuDNN

   2. 使用cmake和nmake进行编译

      ```shell
      cmake -Donnxruntime_BUILD_SHARED_LIB=ON -DONNX_CUSTOM_PROTOC_EXECUTABLE=/path/to/protoc.exe -DCMAKE_INSTALL_PREFIX=../install -Donnxruntime_USE_AVX=ON -Donnxruntime_USE_AVX2=ON -Donnxruntime_USE_AVX512=ON -DCMAKE_BUILD_TYPE=Release -Donnxruntime_USE_CUDA=ON -Donnxruntime_CUDNN_HOME=/path/to/cudnn -G"NMake Makefiles" ../../cmake
      ```

      注意onnxruntime_CUDNN_HOME指向到包含cuda的目录。（cudnn解压出来后，会有一个cuda目录，里面包含include、lib、bin三个目录）

## TensorRT Provider

1. onnxruntime使用1.7.1版本

   ```shell
   git checkout -b v1.7.1 v1.7.1
   ```

2. 下载TensorRT

   TensorRT使用7.1.3.4版本，对应的压缩包为：TensorRT-7.1.3.4.Windows10.x86_64.cuda-11.0.cudnn8.0.zip

   **需要将TensorRT的lib目录添加到PATH中**

3. 下载CUDA和cudnn版本

   依据TensorRT对应的版本，使用CUDA11.0和cudnn8.0。

   **需要将CUDA11.0和cudnn8.0的bin添加到PATH中**

4. 编译onnxruntime

   编辑cmake/CMakeLists.txt文件，在第410行，增加1行

   ```cmake
   string(APPEND CMAKE_CXX_FLAGS " /EHsc /wd4819")
   ```

   忽略4819的警告。如果不增加这一行，在编译onnxruntime的过程中，会将4819的警告示为错误，导致编译失败。

   **使用 x64 Native Tools Command Prompt for VS 2019 的 cmd 窗口进行编译**

   ```bash
   build.bat --config RelWithDebInfo --build_shared_lib --parallel 2 --cmake_generator "Visual Studio 16 2019" --use_tensorrt --tensorrt_home "E:\workspace\cpp_libs\TensorRT-7.1.3.4" --use_cuda --cuda_version 11.0 --cuda_home "C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.0" --cudnn_home "E:\workspace\cpp_libs\cudnn8\cuda" --build_wheel --skip_tests
   ```

   编译出的whl文件在 onnxruntime\build\Windows\RelWithDebInfo\RelWithDebInfo\dist 文件夹中，使用

   ```python
   pip install onnxruntime_gpu_tensorrt-1.7.1-cp38-cp38-win_amd64.whl
   ```

   进行安装

   **注**
   
   1. 编译过程中，如果需要克隆pybind11这个库，由于网络的原因，可能克隆不成功。
   
   可以将cmake/external/onnx/third_party/pybind11拷贝到build/Windows/RelWithDebInfo/pybind11中，并将文件夹重命名为src，即：
   
   build/Windows/RelWithDebInfo/pybind11
   
   ------src
   
   
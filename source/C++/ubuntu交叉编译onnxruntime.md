# ubuntu交叉编译onnxruntime

1. 安装Arm交叉编译工具

   在Arm终端上，执行命令

   ```shell
   dpkg --print-architecture
   ```

   或者终端的arm架构，譬如树莓派3b是 armhf

   之后，在ubuntu系统上，安装**对应**的交叉编译工具

   ```shell
   sudo apt-get install crossbuild-essential-armhf
   ```

   不能安装错误，譬如target是armhf，则不能安装 crossbuild-essential-arm64

   或者在 https://www.linaro.org/downloads/ 下载对应的工具，**下载解压缩结束后，需要将bin目录添加到PATH路径中。**

2. 克隆onnxruntime仓库

   ```shell
   git clone --recursive https://github.com/Microsoft/onnxruntime
   cd onnxruntime/cmake
   mkdir build  # 进行build的目录
   mkdir install # 安装目录
   cd build
   ```

   注：克隆结束后，查看cmake/external/gemmlowp目录，看是否为空。如果是空目录，需要重新克隆gemmlowp仓库。

   ```shell
   cd cmake/external
   rm -rf ./gemmlowp
   git clone https://github.com/google/gemmlowp.git
   ```

3. 下载protoc

   protoc需要和cmake/external/protobuf中的版本保持一致，可以进行下述操作

   ```shell
   cd cmake/external/protobuf
   git checkout -b v3.11.3 v3.11.3
   ```

   下载对应版本的protoc：

    https://github.com/protocolbuffers/protobuf/releases/download/v3.11.3/protoc-3.11.3-linux-x86_64.zip

   解压缩到某个目录

4. 生成Makefile

   在build目录下，创建tool.cmake文件，文件内容为：

   ```cmake
   SET(CMAKE_SYSTEM_NAME Linux)
   SET(CMAKE_SYSTEM_VERSION 1)
   SET(CMAKE_C_COMPILER arm-linux-gnueabihf-gcc)
   SET(CMAKE_CXX_COMPILER arm-linux-gnueabihf-g++)
   SET(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
   SET(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
   SET(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
   SET(CMAKE_FIND_ROOT_PATH_MODE_PACKAGE ONLY)
   ```

   执行命令

   ```shell
   cmake -DCMAKE_TOOLCHAIN_FILE=./tool.cmake -Donnxruntime_BUILD_SHARED_LIB=ON -DCMAKE_INSTALL_PREFIX=../install -DONNX_CUSTOM_PROTOC_EXECUTABLE=/home/lijun/tools/protoc-3.11.3-linux-x86_64/bin/protoc ..
   ```

   注意上面命令中，protoc的路径需要和实际保持一致。CMAKE_INSTALL_PREFIX指定了安装的路径，可以根据需要，配置到指定的目录。

   ```shell
   make -j4
   make install
   ```


### 添加ACL Provider

1. 交叉编译ACL

   ```shell
   sudo apt-get install scons
   git clone https://github.com/ARM-software/ComputeLibrary.git
   cd ComputeLibrary
   git checkout -b v20.02 v20.02  # onnxruntime目前最高支持到20.02
   scons Werror=1 debug=0 asserts=0 neon=1 opencl=1 examples=1 os=linux arch=armv7a -j4  # 编译的文件在build目录下
   ```

2. 交叉编译onnxruntime

   执行命令

   ```shell
   cmake -DCMAKE_TOOLCHAIN_FILE=./tool.cmake -Donnxruntime_BUILD_SHARED_LIB=ON -DCMAKE_INSTALL_PREFIX=../install -DONNX_CUSTOM_PROTOC_EXECUTABLE=/home/lijun/tools/protoc-3.11.3-linux-x86_64/bin/protoc -Donnxruntime_USE_ACL_2002=ON -Donnxruntime_ACL_HOME=/path/to/ComputeLibrary -Donnxruntime_ACL_LIBS=/path/to/build ..
   ```

   注意上面命令中，onnxruntime_ACL_HOME和onnxruntime_ACL_LIBS需要和实际路径保持一致。

   ```shell
   make -j4
   make install
   ```

   


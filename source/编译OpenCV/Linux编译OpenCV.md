# Linux编译OpenCV

1. 克隆OpenCV源码

   Gitee仓库（推荐）：https://gitee.com/cmkyec/opencv.git

   Github仓库：https://github.com/opencv/opencv.git

2. 编译并安装FFmpeg

3. 安装pkg-config（不安装，会导致OpenCV的CMake找不到FFmpeg库）

   ```shell
   sudo apt-get install pkg-config
   ```

4. 安装gtk3（可选，如果不需要显示视频，可以不安装）

   ```shell
   sudo apt-get install libgtk-3-dev
   ```

5. 下载ippicv

   默认在使用cmake-gui生成Makefile过程中，下载第三方库中的IPPICV。
   但是在实际使用中，经常下载ippicv失败。
   查看3rdparty/ippicv/ippicv.cmake文件获取到ippicv包的下载路径，预先使用工具进行下载，存储到某个文件夹下。
   修改3rdparty/ippicv/ippicv.cmake文件中ippicv的下载路径，示例如下：
   ```shell
   "https://raw.githubusercontent.com/opencv/opencv_3rdparty/${IPPICV_COMMIT}/ippicv/"
   替换为
   "file:/home/lijun/workspace/open_source/opencv/"
   ```
   上述路径依据存储的实际情况进行修改，注意最后一个反斜杠**/**是必须的。

6. 使用cmake-gui生成Makefile文件

   ```shell
   BUILD_opencv_python_bindings_generator 取消勾选
   BUILD_opencv_python_tests 取消勾选
   BUILD_opencv_world 勾选
   ```

   如果是在shell中，执行如下命令

   ```shell
   cd opencv_source_folder
   mkdir build
   cd build
   cmake -DBUILD_opencv_python_bindings_generator=OFF -DBUILD_opencv_python_tests=OFF -DBUILD_opencv_world=ON ..
   ```

7. 进行编译以及编译后的安装

   ```shell
   make -j4
   sudo make install
   ```

### 编译OpenCV contrib

1. 克隆OpenCV contrib源码

   Gitee仓库（推荐）：https://gitee.com/cmkyec/opencv_contrib.git

   Github仓库：https://github.com/opencv/opencv_contrib.git

2. 参考contrib仓库README.MD文件进行编译

   编译前，建议将contrib切换成和OpenCV相同的分支。


### 使用CUDA进行加速

参考网址：https://www.pyimagesearch.com/2020/02/03/how-to-use-opencvs-dnn-module-with-nvidia-gpus-cuda-and-cudnn/

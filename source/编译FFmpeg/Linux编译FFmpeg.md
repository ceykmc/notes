# Linux编译FFmpeg

1. 编译x264

   ```shell
   git clone https://code.videolan.org/videolan/x264.git
   cd x264
   git checkout -b stable origin/stable  # 使用稳定版本
   ./configure --enable-shared --enable-static
   make -j4
   ```

2. 克隆FFmpeg源码

   Gitee仓库（推荐）：https://gitee.com/cmkyec/FFmpeg.git

   Github仓库：https://github.com/FFmpeg/FFmpeg.git

3. 切换到最近的Release版本 **（可选）**

   ```shell
   git checkout -b 4.2 origin/release/4.2
   ```

4. 安装yasm**（树莓派不需要）**

   FFMpeg需要依赖yasm编译部分汇编代码

   ```shell
   sudo apt-get install yasm
   ```

5. 执行configure程序，生成Makefile文件

   对于x86架构Linux

   ```shell
   ./configure --enable-shared --enable-gpl --enable-avresample
   ```

   对于树莓派

   ```shell
   ./configure --enable-shared --enable-gpl --enable-libx264 --enable-encoder=libx264 --enable-decoder=h264 --enable-omx --enable-omx-rpi --enable-encoder=h264_omx --enable-mmal --enable-decoder=h264_mmal --enable-hwaccel=h264_mmal
   ```

6. 进行编译以及编译后的安装

   ```shell
   make -j4
   sudo make install
   ```

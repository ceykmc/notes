# Linux下编译FFmpeg

1. 克隆FFmpeg源码

   Gitee仓库（推荐）：https://gitee.com/cmkyec/FFmpeg.git

   Github仓库：https://github.com/FFmpeg/FFmpeg.git

2. 切换到最近的Release版本 **（可选）**

   ```shell
   git checkout -b 4.2 origin/release/4.2
   ```

3. 安装yasm

   FFMpeg需要依赖yasm编译部分汇编代码

   ```shell
   sudo apt-get install yasm
   ```

4. 执行configure程序，生成Makefile文件

   ```shell
   ./configure --enable-shared --enable-avresample
   ```

5. 进行编译以及编译后的安装

   ```shell
   make -j4
   sudo make install
   ```

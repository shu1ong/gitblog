# [Orb-slam 2失败的编译过程](https://github.com/shu1ong/gitblog/issues/6)

CMake Error at /usr/share/cmake-3.16/Modules/FindPackageHandleStandardArgs.cmake:146 (message):
Could NOT find PythonInterp: Found unsuitable version "2.7.18", but   required is at least "3" 

这里是python的调用问题

因为ros1是支持python 2.0的 会有混用的情况的出现

目前的解决方案是在build目录下修改camkecache文件里
usr/bin/python
改为 
usr/bin/python3

---

/usr/bin/ld: /usr/lib/x86_64-linux-gnu/libopencv_imgproc.so.4.2.0: error adding symbols: DSO missing from command line
collect2: error: ld returned 1 exit status
make[2]: *** [CMakeFiles/MonoAR.dir/build.make:238：../MonoAR] 错误 1
make[1]: *** [CMakeFiles/Makefile2:487：CMakeFiles/MonoAR.dir/all] 错误 2
make: *** [Makefile:130：all] 错误 2


---

https://blog.csdn.net/lzRush/article/details/84579692

---

如果你是以CMakeList来管理项目，那么解决方法（以我的错误为例）如下：

1.打开CMakeList.txt

2.在CMakeList.txt中添加

//我缺少的是libopencv_imgproc.so.2.4这个链接库，我们只需要在CMakeList中定向链接这个库即可

set(LIB_OPENCV_IMGPROC_DIR /usr/local/lib)   //LIB_OPENCV_IMGPROC_DIR是变量名，可以随意写
add_library(libopencv_imgproc SHARED IMPORTED)
set_target_properties(libopencv_imgproc PROPERTIES IMPORTED_LOCATION ${LIB_OPENCV_IMGPROC_DIR}/libopencv_imgproc.so.2.4.9)

target_link_libraries(demo  libopencv_imgproc )//注意这条语句要放在add_executable的后面
————————————————
版权声明：本文为CSDN博主「御名方守矢-」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/lzRush/article/details/84579692

---

这个办法其实没有解决 最后解决的办法是修改了opencv的版本
参照
https://www.yuque.com/xtdrone/manual_cn/vslam
由于Noetic自带的OpenCV版本是4.2.0，在编译时需要先修改CMakeList.txt

---

ORB-SLAM2 Copyright (C) 2014-2016 Raul Mur-Artal, University of Zaragoza.
This program comes with ABSOLUTELY NO WARRANTY;
This is free software, and you are welcome to redistribute it
under certain conditions. See LICENSE.txt.

Input sensor was set to: Stereo
Segmentation fault (core dumped)


---

问题是由 Eigen 引起的。使用版本 3.2.10 而不是 3.3.X。

---

#define EIGEN_WORLD_VERSION 3
#define EIGEN_MAJOR_VERSION 3
#define EIGEN_MINOR_VERSION 7

---

我的版本是用的3.3.7的

---

https://blog.csdn.net/qq_45401419/article/details/118358687

---

#删除之前版本
locate eigen3
sudo rm -rf /usr/include/eigen3
sudo rm -rf /usr/lib/cmake/eigen3
sudo rm -rf /usr/local/include/eigen3
sudo rm -rf /usr/share/doc/libeigen3-dev 
sudo rm -rf /usr/local/share/pkgconfig/eigen3.pc /usr/share/pkgconfig/eigen3.pc /var/lib/dpkg/info/libeigen3-dev.list /var/lib/dpkg/info/libeigen3-dev.md5sums
sudo rm -rf /usr/local/lib/pkgconfig/eigen3.pc
sudo rm -rf /usr/local/share/eigen3
pkg-config --modversion eigen3
# 下载新的编译安装
wget https://gitlab.com/libeigen/eigen/-/archive/3.3.0/eigen-3.3.0.zip
unzip eigen-3.3.0.zip
cd eigen-3.3.0
mkdir build
cd build
cmake ..
sudo make install
sudo cp -r /usr/local/include/eigen3 /usr/include 
# 检测当前版本
pkg-config --modversion eigen3

# [vins-fusion配置](https://github.com/shu1ong/gitblog/issues/7)

依赖
Ceres Solver
据说最好安装的版本是1.14.0
新的版本和eigen3.3.7好像不太匹配容易报错
安装指南 以及ceres的依赖
glog
https://zhuanlan.zhihu.com/p/459061947
ceres 1.14.0
https://zhuanlan.zhihu.com/p/460685629

---

靠 昨天装orb-slam2该了eigen3的版本 现在报错找不到包了

---

CMake Error at /usr/share/dart/cmake/DARTFindEigen3.cmake:12 (find_package):
  Could not find a package configuration file provided by "Eigen3" with any
  of the following names:

    Eigen3Config.cmake
    eigen3-config.cmake

---

![image](https://user-images.githubusercontent.com/127008177/224891038-e235f7e2-3a14-4f5a-84b7-c20bc74e42ff.png)
能够检验到包
考虑要不要改回去

---

![image](https://user-images.githubusercontent.com/127008177/224891603-83593070-25b0-47b3-850e-b4db10c111a1.png)
已经可以熟练的卸载重新安装了

---

![image](https://user-images.githubusercontent.com/127008177/224895429-9102bca5-d11e-41c0-a551-82585d1b9dab.png)
ceres又不行了，重新编译安装试试

---

成功解决以及新的问题：
![image](https://user-images.githubusercontent.com/127008177/224897678-a28b43e8-5fac-4ad4-ad4d-d87ad445f8e1.png)


---

本着缺什么补什么的精神
sudo apt-get install ros-noetic-cmake-modules

---

![image](https://user-images.githubusercontent.com/127008177/224903007-2ac48d3a-fa3e-44ec-8a61-76c5b5bed62e.png)
可以成功编译到84%

---

第一次报错的地方在这
![image](https://user-images.githubusercontent.com/127008177/224903863-b30f329e-3385-4e6c-8146-8453abd23325.png)


---

这个问题为我有印象，在orb-slam2 中的配置里有提到过如何解决。
![image](https://user-images.githubusercontent.com/127008177/224904045-85ee2548-78e4-4c1c-8230-f96dc1b2fcdd.png)
https://www.yuque.com/xtdrone/manual_cn/vslam

---

加上代码还是同样的报错
冲浪回来说是opencv2到后续版本的更新一些写法调用上出现了不兼容的情况：
参照
https://blog.csdn.net/qq_45945548/article/details/124754325
![image](https://user-images.githubusercontent.com/127008177/224906215-727bb0e9-d573-4c87-9c4a-922a2f24125f.png)


---

本着后面还有一堆报错 我觉得改opencv库的方式会好用一点
先看看目前自己用的啥库
```
pkg-config opencv --modversion
```
3.4.16
关于opencv的多版本安装和切换的问题
[切换版本](https://www.cnblogs.com/dylancao/p/9493061.html)
[多版本安装](https://blog.csdn.net/learning_tortosie/article/details/80594399)

---

emmm
好像不需要重装了？
https://blog.csdn.net/qq_40904854/article/details/124634997
![image](https://user-images.githubusercontent.com/127008177/224909463-302bc459-5c79-4642-8ce5-38e4919de453.png)


---

```shell
/home/jsl/catkin_ws/src/VINS-Fusion/loop_fusion/src/keyframe.cpp: In member function ‘bool KeyFrame::findConnection(KeyFrame*)’:
/home/jsl/catkin_ws/src/VINS-Fusion/loop_fusion/src/keyframe.cpp:455:130: error: ‘CV_FONT_HERSHEY_SIMPLEX’ was not declared in this scope
  455 |              putText(notation, "current frame: " + to_string(index) + "  sequence: " + to_string(sequence), cv::Point2f(20, 30), CV_FONT_HERSHEY_SIMPLEX, 1, cv::Scalar(255), 3);
      |                                                                                                                                  ^~~~~~~~~~~~~~~~~~~~~~~
[ 92%] Linking CXX shared library /home/jsl/catkin_ws/devel/lib/libgazebo_ros_openni_kinect.so
/home/jsl/catkin_ws/src/VINS-Fusion/loop_fusion/src/pose_graph.cpp: In member function ‘int PoseGraph::detectLoop(KeyFrame*, int)’:
/home/jsl/catkin_ws/src/VINS-Fusion/loop_fusion/src/pose_graph.cpp:343:97: error: ‘CV_FONT_HERSHEY_SIMPLEX’ was not declared in this scope
  343 |         putText(compressed_image, "feature_num:" + to_string(feature_num), cv::Point2f(10, 10), CV_FONT_HERSHEY_SIMPLEX, 0.4, cv::Scalar(255));
      |                                                                                                 ^~~~~~~~~~~~~~~~~~~~~~~
/home/jsl/catkin_ws/src/VINS-Fusion/loop_fusion/src/pose_graph.cpp:364:101: error: ‘CV_FONT_HERSHEY_SIMPLEX’ was not declared in this scope
  364 |             putText(loop_result, "neighbour score:" + to_string(ret[0].Score), cv::Point2f(10, 50), CV_FONT_HERSHEY_SIMPLEX, 0.5, cv::Scalar(255));
      |                                                                                                     ^~~~~~~~~~~~~~~~~~~~~~~
/home/jsl/catkin_ws/src/VINS-Fusion/loop_fusion/src/pose_graph.cpp:374:130: error: ‘CV_FONT_HERSHEY_SIMPLEX’ was not declared in this scope
  374 |             putText(tmp_image, "index:  " + to_string(tmp_index) + "loop score:" + to_string(ret[i].Score), cv::Point2f(10, 50), CV_FONT_HERSHEY_SIMPLEX, 0.5, cv::Scalar(255));
      |                                                                                                                                  ^~~~~~~~~~~~~~~~~~~~~~~
/home/jsl/catkin_ws/src/VINS-Fusion/loop_fusion/src/pose_graph.cpp:391:102: error: ‘CV_FONT_HERSHEY_SIMPLEX’ was not declared in this scope
  391 |                     putText(tmp_image, "loop score:" + to_string(ret[i].Score), cv::Point2f(10, 50), CV_FONT_HERSHEY_SIMPLEX, 0.4, cv::Scalar(255));
      |                                                                                                      ^~~~~~~~~~~~~~~~~~~~~~~
/home/jsl/catkin_ws/src/VINS-Fusion/loop_fusion/src/pose_graph.cpp:409:47: warning: comparison of integer expressions of different signedness: ‘DBoW2::EntryId’ {aka ‘unsigned int’} and ‘int’ [-Wsign-compare]
  409 |             if (min_index == -1 || (ret[i].Id < min_index && ret[i].Score > 0.015))
/home/jsl/catkin_ws/src/VINS-Fusion/loop_fusion/src/pose_graph.cpp: In member function ‘void PoseGraph::addKeyFrameIntoVoc(KeyFrame*)’:
/home/jsl/catkin_ws/src/VINS-Fusion/loop_fusion/src/pose_graph.cpp:427:97: error: ‘CV_FONT_HERSHEY_SIMPLEX’ was not declared in this scope
  427 |         putText(compressed_image, "feature_num:" + to_string(feature_num), cv::Point2f(10, 10), CV_FONT_HERSHEY_SIMPLEX, 0.4, cv::Scalar(255));
```

---

大跌写的教程
https://zhuanlan.zhihu.com/p/432167383
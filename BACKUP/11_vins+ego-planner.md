# [vins+ego-planner](https://github.com/shu1ong/gitblog/issues/11)

启动gazebo仿真
```
roslaunch px4 indoor1.launch
```
启动vins（我把vins的rviz注释掉了）
```
cd ~/catkin_ws
bash scripts/xtdrone_run_vio.sh
```
话题类型转化：vins发布位置
```
cd ~/XTDrone/sensing/slam/vio
python vins_transfer.py iris 0
```
构建键盘通讯
```
cd ~/XTDrone/communication
python multirotor_communication.py iris 0 
```
唤起键盘控制菜单
```
cd ~/XTDrone/control/keyboard
python multirotor_keyboard_control.py iris 1 vel
```

ego-planner 启动
转换坐标
```
cd ~/XTDrone/motion_planning/3d
python ego_transfer.py iris 0
```
启动rviz
```
cd ~/XTDrone/motion_planning/3d
rviz -d ego_rviz.rviz
```


启动ego_planner
```
cd ~/catkin_ws/src
roslaunch ego_planner single_uav.launch
```


---

起飞后切hover会有点奇怪，会有一个抬头的动作，不知道是不是加速度设置过大的原因

---

```
killall gzserver
killall gzclient
```

---

墙面的识别需要复杂的纹理支持


---

记得在launch文件中修改飞机的相机模型，把特征相机（stereo）改成深度（realsence）相机，和相机发布的话题有关，也与ego-planner接受的数据有关

---

目标点的坐标是基于机体系而言的，不是全局坐标系

---

https://blog.csdn.net/qq_39779233/article/details/119720235

---

允许最大的加速度调大的话更容易过
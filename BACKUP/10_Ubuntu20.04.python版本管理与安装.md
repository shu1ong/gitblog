# [Ubuntu20.04 python版本管理与安装](https://github.com/shu1ong/gitblog/issues/10)

pip2的安装
https://zhuanlan.zhihu.com/p/137114974

---

```
  WARNING: The scripts f2py, f2py2 and f2py2.7 are installed in '/home/j/.local/bin' which is not on PATH.
  Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
  WARNING: The scripts pyserial-miniterm and pyserial-ports are installed in '/home/j/.local/bin' which is not on PATH.
  Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
  WARNING: The scripts ulog2csv, ulog2kml, ulog_extract_gps_dump, ulog_info, ulog_messages and ulog_params are installed in '/home/j/.local/bin' which is not on PATH.
  Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
```

---

https://blog.csdn.net/qq_44739762/article/details/107442241

---

for pyhton3
```
Traceback (most recent call last):
  File "multirotor_communication.py", line 6, in <module>
    from pyquaternion import Quaternion
ModuleNotFoundError: No module named 'pyquaternion'
```

---

for pyhton2
```
Traceback (most recent call last):
  File "multirotor_communication.py", line 1, in <module>
    import rospy
  File "/opt/ros/noetic/lib/python3/dist-packages/rospy/__init__.py", line 49, in <module>
    from .client import spin, myargv, init_node, \
  File "/opt/ros/noetic/lib/python3/dist-packages/rospy/client.py", line 52, in <module>
    import roslib
  File "/opt/ros/noetic/lib/python3/dist-packages/roslib/__init__.py", line 50, in <module>
    from roslib.launcher import load_manifest  # noqa: F401
  File "/opt/ros/noetic/lib/python3/dist-packages/roslib/launcher.py", line 42, in <module>
    import rospkg
ImportError: No module named rospkg
```
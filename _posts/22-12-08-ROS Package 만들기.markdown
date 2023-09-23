---
layout: post
title:  "ROS Package 만들기"
date:   2022-12-08 21:03:36 +0530
categories: ROS
---

```
cd ~/catkin_ws/src
```

```
catkin_create_pkg <package_name> [depend1] [depend2] [depend3]
```

```
catkin_create_pkg pkg_name std_msgs rospy roscpp
```

workspace 초기화
```
catkin_init_workspace
```

빌드
```
catkin_make
```

배쉬 등록
```
~/catkin_ws/devel/setup.bash
```
path planner when a goal from rviz is received, working with my ORB_SLAM2.
```
rosrun rviz rviz/blind_aid.rviz
roslaunch path_planner blind_aid.launch
rosrun ORB_SLAM2 RGBD ./Vocabulary/ORBvoc.txt Examples/RGB-D/TUM1.yaml
```

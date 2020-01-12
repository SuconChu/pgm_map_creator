# pgm_map_creator
Create pgm map from Gazebo world file for ROS localization
## Environment
Tested on Ubuntu 16.04, ROS Kinetic, Boost 1.58

## Usage
### 中文为准
### Add the package to your workspace
0. Create a catkin workspace `creator`
1. Clone the package to the src folder
2. `cd ~/creator`  `catkin_make` and `source ~/creator/devel/setup.bash`

### Add the map and insert the plugin
1. Add your world file to THE PACKAGE's world folder
2. Add this line at the end of the world file, before `</world>` tag:
`<plugin filename="libcollision_map_creator.so" name="collision_map_creator"/>`.
Make sure you specify the absolute path to the library so gazebo knows how to find it, unless the lib is at the same directory with the world.使用绝对路径.不知道在哪里可以先搜索找到路径.

### Create the pgm map file
1. Open a terminal, run gzerver with the map file
`gzserver /home/z/creator/src/pgm_map_creator/world/target_world_file_name.word`,specify the absolute path.出现 Subscribing to: ~/collision_map/command 则表明正常运行.
2. Open another terminal, launch the request_publisher node
`roslaunch pgm_map_creator request_publisher.launch`
3. Wait for the plugin to generate map. It will be located in the map folder

## 中文

### 编辑文件
已更改，无需再改

msgs/CMakeLists.txt
	`set (msgs  collision_map_request.proto
	  ${PROTOBUF_IMPORT_DIRS}/vector2d.proto
	  ${PROTOBUF_IMPORT_DIRS}/header.proto
	  ${PROTOBUF_IMPORT_DIRS}/time.proto
	)`
改为
	`set (msgs
	  collision_map_request.proto
	)`


### 编译  `catkin_make`

### export环境变量 `source ~/creator/devel/setup.bash`

### 编辑 *.world 文件
在`</world>`前 添加插件 `<plugin filename="/home/z/creator/devel/lib/libcollision_map_creator.so" name="collision_map_creator"/>`,要使用绝对路径

### 创建pgm文件
打开一个终端
`gzserver ~/creator/src/pgm_map_creator/world/udacity_mtv`
出现 `Subscribing to: ~/collision_map/command` 则表明正常运行

再打开一个终端
`roslaunch pgm_map_creator request_publisher.launch`


export环境变量
Lastly, don't forget to export the environment variable as suggested in the tutorial
`export GAZEBO_PLUGIN_PATH=$GAZEBO_PLUGIN_PATH:collision_map_creator_plugin/build`


## Map Properties
Currently, please update the argument value in launch/request_publisher.launch file.

## Acknowledgements
[Gazebo Custom Messages](http://gazebosim.org/wiki/Tutorials/1.9/custom_messages)
[Gazebo Perfect Map Generator](https://github.com/koenlek/ros_lemtomap/tree/154c782cf8feb9112bc928e33a59728ca2192489/st_gazebo_perfect_map_generator)


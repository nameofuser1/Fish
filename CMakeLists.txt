cmake_minimum_required(VERSION 2.8.3)
project(fish)

## Compile as C++11, supported in ROS Kinetic and newer
add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  geometry_msgs
  roscpp
  rospy
  std_msgs
)

## System dependencies are found with CMake's conventions
# find_package(Boost REQUIRED COMPONENTS system)
find_package(catkin REQUIRED
  COMPONENTS
    pluginlib
    geometric_shapes
    xacro
)


## XACRO
# Robot environments
find_package(xacro REQUIRED)

file(GLOB xacro_envs_files ${CMAKE_CURRENT_SOURCE_DIR}/urdf/*.urdf.xacro)

foreach(it ${xacro_envs_files})
	string(REGEX MATCH "(.*)[.]xacro$" unused ${it})
	set(output_filename ${CMAKE_MATCH_1})

	xacro_add_xacro_file(${it} ${output_filename} INORDER)
	list(APPEND envs_files ${output_filename})
endforeach(it)

add_custom_target(media_files ALL DEPENDS ${envs_files})
include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)


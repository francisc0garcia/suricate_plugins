cmake_minimum_required(VERSION 2.8.1)
project(suricate_plugins)

# C++14:
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++1y")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${GAZEBO_CXX_FLAGS}")

#set(CMAKE_CXX_COMPILER "clang++")
#set(CMAKE_C_COMPILER "clang")

find_package(catkin REQUIRED COMPONENTS
        roscpp
        rospy
        std_msgs
        geometry_msgs
        sensor_msgs
        dynamic_reconfigure
        image_transport
        message_filters
        tf
        nav_msgs
        visualization_msgs
        gazebo_ros
        )

# Depend on system install of Gazebo
find_package(gazebo REQUIRED)

catkin_package(
        INCLUDE_DIRS include
        LIBRARIES suricate_plugins gazebo_ros_diff_drive
        CATKIN_DEPENDS roscpp rospy std_msgs geometry_msgs sensor_msgs
        DEPENDS  roscpp gazebo_ros gazebo  SDF geometry_msgs
)

include_directories(
        include
        ${catkin_INCLUDE_DIRS}
)

set(EXECUTABLE_OUTPUT_PATH  ${PROJECT_SOURCE_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib )

link_directories(${GAZEBO_LIBRARY_DIRS})
include_directories(${Boost_INCLUDE_DIR} ${catkin_INCLUDE_DIRS} ${GAZEBO_INCLUDE_DIRS})

add_library(wrench_driver src/gazebo_plugins/wrench_driver.cpp)
target_link_libraries(wrench_driver gazebo_ros_utils ${catkin_LIBRARIES} ${GAZEBO_LIBRARIES} ${Boost_LIBRARIES})

add_library(gazebo_ros_utils src/gazebo_plugins/gazebo_ros_utils.cpp)
target_link_libraries(gazebo_ros_utils ${GAZEBO_LIBRARIES} ${catkin_LIBRARIES} ${Boost_LIBRARIES})
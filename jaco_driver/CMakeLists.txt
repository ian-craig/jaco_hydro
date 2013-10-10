cmake_minimum_required(VERSION 2.8.3)
project(jaco_driver)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  actionlib
  roscpp
  rospy
  message_generation
  std_msgs
  geometry_msgs
  nav_msgs
  sensor_msgs
  xacro
  tf
  actionlib_msgs
)


## System dependencies are found with CMake's conventions
# find_package(Boost REQUIRED COMPONENTS system)

add_action_files(DIRECTORY action FILES 
  ArmJointAngles.action
  ArmPose.action
  SetFingersPosition.action
)


## Uncomment this if the package has a setup.py. This macro ensures
## modules and global scripts declared therein get installed
## See http://ros.org/doc/api/catkin/html/user_guide/setup_dot_py.html
# catkin_python_setup()

#######################################
## Declare ROS messages and services ##
#######################################

## Generate messages in the 'msg' folder
add_message_files(
  FILES
  FingerPosition.msg
  JointAngles.msg
  JointVelocity.msg
)

## Generate services in the 'srv' folder
add_service_files(
  FILES
  HomeArm.srv
  Start.srv
  Stop.srv
)

## Generate added messages and services with any dependencies listed here
generate_messages(
  DEPENDENCIES
  nav_msgs
  geometry_msgs
  std_msgs
  actionlib_msgs
)

###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if you package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES jaco_driver
   CATKIN_DEPENDS actionlib roscpp tf xacro std_msgs sensor_msgs nav_msgs geometry_msgs roscpp rospy
#  DEPENDS system_lib
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
# include_directories(include)
include_directories(
  ${catkin_INCLUDE_DIRS}
)

## Declare a cpp library
# add_library(jaco_driver
#   src/${PROJECT_NAME}/jaco_driver.cpp
# )

add_library(jacoapi src/jaco_api.cpp)
add_library(jacokinematics src/jaco_arm_kinematics.cpp)

add_library(jaco_arm_driver_lib src/jaco_arm.cpp src/jaco_comm.cpp src/jaco_pose_action.cpp src/jaco_angles_action.cpp src/jaco_fingers_action.cpp src/jaco_types.cpp)

## Declare a cpp executable
# add_executable(jaco_driver_node src/jaco_driver_node.cpp)

add_executable(jaco_arm_driver src/nodes/jaco_arm_driver.cpp )
#add_executable(jaco_tf_updater src/nodes/jaco_tf_updater.cpp )


## Add cmake target dependencies of the executable/library
## as an example, message headers may need to be generated before nodes
# add_dependencies(jaco_driver_node jaco_driver_generate_messages_cpp)

add_dependencies(jaco_arm_driver jaco_driver_generate_messages_cpp)
#add_dependencies(jaco_tf_updater jaco_driver_generate_messages_cpp)

## Specify libraries to link a library or executable target against
# target_link_libraries(jaco_driver_node
#   ${catkin_LIBRARIES}
# )

target_link_libraries(jaco_arm_driver_lib jacoapi ${catkin_LIBRARIES})
target_link_libraries(jaco_arm_driver jaco_arm_driver_lib jacoapi ${catkin_LIBRARIES})
#target_link_libraries(jaco_tf_updater jacokinematics ${catkin_LIBRARIES})

#############
## Install ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html

## Mark executable scripts (Python etc.) for installation
## in contrast to setup.py, you can choose the destination
# install(PROGRAMS
#   scripts/my_python_script
#   DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark executables and/or libraries for installation
# install(TARGETS jaco_driver jaco_driver_node
#   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark cpp header files for installation
# install(DIRECTORY include/${PROJECT_NAME}/
#   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#   FILES_MATCHING PATTERN "*.h"
#   PATTERN ".svn" EXCLUDE
# )

## Mark other files for installation (e.g. launch and bag files, etc.)
# install(FILES
#   # myfile1
#   # myfile2
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
# )

#############
## Testing ##
#############

## Add gtest based cpp test target and link libraries
# catkin_add_gtest(${PROJECT_NAME}-test test/test_jaco_driver.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)

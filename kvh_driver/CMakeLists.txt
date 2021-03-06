cmake_minimum_required(VERSION 2.8.3)
project(kvh_driver)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  device_driver_base
  dynamic_reconfigure
  nav_msgs
  roscpp
  roslib
  rospy
  sensor_msgs
  std_msgs
)
generate_dynamic_reconfigure_options(
  cfg/KVHDriver.cfg
)
add_dependencies(${PROJECT_NAME}_gencfg device_driver_base_generate_messages)

## System dependencies are found with CMake's conventions
# find_package(Boost REQUIRED COMPONENTS system)


## Uncomment this if the package has a setup.py. This macro ensures
## modules and global scripts declared therein get installed
## See http://ros.org/doc/api/catkin/html/user_guide/setup_dot_py.html
# catkin_python_setup()

#######################################
## Declare ROS messages and services ##
#######################################

## Generate messages in the 'msg' folder
# add_message_files(
#   FILES
#   Message1.msg
#   Message2.msg
# )

## Generate services in the 'srv' folder
# add_service_files(
#   FILES
#   Service1.srv
#   Service2.srv
# )

## Generate added messages and services with any dependencies listed here
# generate_messages(
#   DEPENDENCIES
#   nav_msgs#   sensor_msgs#   std_msgs
# )

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
  INCLUDE_DIRS include/kvh_driver
#  LIBRARIES kvh_driver
#  CATKIN_DEPENDS device_driver_base dynamic_reconfigure nav_msgs roscpp roslib rospy sensor_msgs std_msgs
#  DEPENDS system_lib
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(include)
include_directories(
  ${catkin_INCLUDE_DIRS}
)

## Declare a cpp library
add_library(${PROJECT_NAME}Driver src/helpers/kvh_imu.cpp)

## Declare a cpp executable
add_executable(kvh_driver_node src/nodes/kvh_driver_node.cpp)
add_dependencies(kvh_driver_node ${PROJECT_NAME}_gencfg) 
target_link_libraries(kvh_driver_node ${PROJECT_NAME}Driver ${catkin_LIBRARIES})


add_executable(serialtest src/util/serialtest.cpp)
target_link_libraries(serialtest ${PROJECT_NAME}Driver ${catkin_LIBRARIES})

add_executable(stream_viewer src/util/stream_viewer.cpp)
target_link_libraries(stream_viewer ${PROJECT_NAME}Driver ${catkin_LIBRARIES})

add_executable(bit src/util/bit.cpp)
target_link_libraries(bit ${PROJECT_NAME}Driver ${catkin_LIBRARIES})

add_executable(config src/util/config.cpp)
target_link_libraries(config ${PROJECT_NAME}Driver ${catkin_LIBRARIES})

add_executable(drift_test src/util/drift_test.cpp)
target_link_libraries(drift_test ${PROJECT_NAME}Driver ${catkin_LIBRARIES})



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
# install(TARGETS kvh_driver kvh_driver_node
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
# catkin_add_gtest(${PROJECT_NAME}-test test/test_kvh_driver.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)

cmake_minimum_required(VERSION 2.8.3)
project(nortek_dvl)

## Compile as C++11, supported in ROS Kinetic and newer
add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  geometry_msgs
  roscpp
  std_msgs
  message_generation
)

################################################
## Declare ROS messages, services and actions ##
################################################

## Generate messages in the 'msg' folder
add_message_files(
  FILES
  Dvl.msg
  DvlStatus.msg
)

## Generate added messages and services with any dependencies listed here
generate_messages(
  DEPENDENCIES
  geometry_msgs 
  std_msgs
)

###################################
## catkin specific configuration ##
###################################

catkin_package(
  INCLUDE_DIRS include
  CATKIN_DEPENDS message_runtime 
)
##catkin_package()

###########
## Build ##
###########

## Specify additional locations of header files
include_directories(
  include
  tacopie/includes
  ${catkin_INCLUDE_DIRS}
)

# build tacopie library
OPTION(BUILD_EXAMPLES "examples for taco pie" OFF)
add_subdirectory(tacopie)

# build DVL node
add_executable(${PROJECT_NAME} src/dvl_node.cpp src/dvl.cpp)
add_dependencies(${PROJECT_NAME} ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS} tacopie)
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES} tacopie)

install(TARGETS ${PROJECT_NAME}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY include/
        DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}/${PROJECT_NAME}
)

install(DIRECTORY launch/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch
)
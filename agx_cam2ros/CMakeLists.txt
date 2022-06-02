cmake_minimum_required(VERSION 3.0.2)
project(agx_cam2ros)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  sensor_msgs
  std_msgs
)

set(CLASSES "/usr/src/jetson_multimedia_api/samples/common/classes")
set(ALGO "/usr/src/jetson_multimedia_api/samples/common/algorithm/cuda")

find_library(LIB_PTHREAD name pthread PATHS "/usr/local/cuda/lib64")
find_library(LIB_V4L2 name v4l2 PATHS "/usr/local/cuda/lib64")
find_library(LIB_EGL name EGL PATHS "/usr/local/cuda/lib64")
find_library(LIB_GLESv2 name GLESv2 PATHS "/usr/local/cuda/lib64")
find_library(LIB_X11 name X11 PATHS "/usr/local/cuda/lib64")
find_library(LIB_NVBUF_UTILS name nvbuf_utils PATHS "/usr/lib/aarch64-linux-gnu/tegra")
find_library(LIB_NVJPEG name nvjpeg PATHS "/usr/lib/aarch64-linux-gnu/tegra")
find_library(LIB_NVOSD name nvosd PATHS "/usr/lib/aarch64-linux-gnu/tegra")
find_library(LIB_DRM name drm PATHS "/usr/local/cuda/lib64")
find_library(LIB_CUDA name cuda PATHS "/usr/local/cuda/lib64")
find_library(LIB_CUDART name cudart PATHS "/usr/local/cuda/lib64")
find_library(LIB_NVINFER name nvinfer PATHS "/usr/lib/aarch64-linux-gnu/tegra")
find_library(LIB_NVPARSERS name nvparsers PATHS "/usr/lib/aarch64-linux-gnu/tegra")

catkin_package(
  INCLUDE_DIRS include
#  LIBRARIES agx_cam2ros
#  CATKIN_DEPENDS roscpp sensor_msgs std_msgs
#  DEPENDS system_lib
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  /usr/src/jetson_multimedia_api/include
  /usr/src/jetson_multimedia_api/include/libjpeg-8b
  /usr/src/jetson_multimedia_api/samples/common/algorithm/cuda
  /usr/src/jetson_multimedia_api/samples/common/algorithm/trt
  /usr/src/jetson_multimedia_api/samples/common/classes
  /usr/local/cuda/include
  /usr/include/libdrm
  /usr/include/opencv4
  /usr/include/aarch64-linux-gnu
)

add_executable(${PROJECT_NAME}_node 
    src/agx_cam2ros.cpp
    ${CLASSES}/NvEglRenderer.o
    ${CLASSES}/NvUtils.o
    ${ALGO}/NvCudaProc.o
    ${ALGO}/NvAnalysis.o
    ${CLASSES}/NvJpegDecoder.o
    ${CLASSES}/NvLogging.o
    ${CLASSES}/NvElement.o
    ${CLASSES}/NvElementProfiler.o
    ${CLASSES}/NvBuffer.o
    ${CLASSES}/NvJpegEncoder.o)
    
  target_link_libraries(${PROJECT_NAME}_node 
${LIB_CUDA}
${LIB_PTHREAD}
${LIB_V4L2}
${LIB_NVBUF_UTILS}
${LIB_EGL}
${LIB_NVJPEG}
${LIB_GLESv2}
${LIB_X11}
${LIB_DRM}
${LIB_CUDART}
${LIB_NVOSD}
${LIB_NVINFER}
${LIB_NVPARSERS}
${catkin_LIBRARIES}
)

## Rename C++ executable without prefix
## The above recommended prefix causes long target names, the following renames the
## target back to the shorter version for ease of user use
## e.g. "rosrun someones_pkg node" instead of "rosrun someones_pkg someones_pkg_node"
# set_target_properties(${PROJECT_NAME}_node PROPERTIES OUTPUT_NAME node PREFIX "")

## Add cmake target dependencies of the executable
## same as for the library above
# add_dependencies(${PROJECT_NAME}_node ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

## Specify libraries to link a library or executable target against
# target_link_libraries(${PROJECT_NAME}_node
#   ${catkin_LIBRARIES}
# )

#############
## Install ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html

## Mark executable scripts (Python etc.) for installation
## in contrast to setup.py, you can choose the destination
# catkin_install_python(PROGRAMS
#   scripts/my_python_script
#   DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark executables for installation
## See http://docs.ros.org/melodic/api/catkin/html/howto/format1/building_executables.html
# install(TARGETS ${PROJECT_NAME}_node
#   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark libraries for installation
## See http://docs.ros.org/melodic/api/catkin/html/howto/format1/building_libraries.html
# install(TARGETS ${PROJECT_NAME}
#   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   RUNTIME DESTINATION ${CATKIN_GLOBAL_BIN_DESTINATION}
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
# catkin_add_gtest(${PROJECT_NAME}-test test/test_agx_cam2ros.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)

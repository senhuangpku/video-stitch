# 指定cmake的最低版本
cmake_minimum_required(VERSION 3.1)
# 该工程名字
project( VedioStich )
# 支持C++11特性
add_definitions(-std=c++11)

INCLUDE(/usr/share/cmake-3.16/Modules/FindCUDA.cmake)
INCLUDE_DIRECTORIES(include)
# 寻找安装的OpenCV库
find_package( OpenCV REQUIRED )
FIND_PACKAGE(CUDA REQUIRED)
SET(CUDA_NVCC_FLAGS -gencode arch=compute_75,code=sm_75)
# 添加OpenCV的头文件
include_directories( ${OpenCV_INCLUDE_DIRS} )
add_library( hello apap.cpp blender.cpp exposure_compensator.cpp stdafx.cpp video_stitcher.cpp )
# 生成可执行文件imageStitch
CUDA_ADD_EXECUTABLE(videostitch video_stitch.cpp composer.cu)
# 链接OpenCV的库
target_link_libraries( videostitch ${OpenCV_LIBS}  hello)
cmake_minimum_required(VERSION 3.15)

project(rknn_model_zoo_utils)

# 设置 C++ 标准
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# 设置 OpenCV
set(OpenCV_DIR /usr)
find_package(OpenCV REQUIRED)
message(STATUS "OpenCV_DIR=${OpenCV_DIR}")
message(STATUS "OpenCV_LIBS=${OpenCV_LIBS}")

# 添加库文件
add_library(fileutils STATIC
    file_utils.c
)
target_include_directories(fileutils PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
)

add_library(imagedrawing STATIC
    image_drawing.c
)
target_include_directories(imagedrawing PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
)

# only RGA on rv1106 and rk3588 support handle
if (TARGET_SOC STREQUAL "rv1106" OR TARGET_SOC STREQUAL "rk3588")
    add_definitions(-DLIBRGA_IM2D_HANDLE)
endif()

# 将 image_utils.c 改为 image_utils.cpp
add_library(imageutils STATIC
    image_utils.cpp
)
target_include_directories(imageutils PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
)

# 链接库
target_link_libraries(imageutils
    ${OpenCV_LIBS}
    ${LIBRGA}
)

target_include_directories(imageutils PUBLIC
    ${STB_INCLUDES}
    ${LIBJPEG_INCLUDES}
    ${LIBRGA_INCLUDES}
)





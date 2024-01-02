cmake_minimum_required(VERSION 3.20)

project(jetson-multimedia-api)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

find_package(CUDAToolkit REQUIRED)
find_package(NVMMAPI REQUIRED)

include("BuildSettings.cmake")

# TODO remove this once cleared
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} \
    -Wno-sign-conversion")

# A non-trivial portion of the Jetson Multimedia API is distributed as source code with no compiled .so
# library. We build it here as a shared library
add_library(
  jetson-multimedia-api SHARED
  src/NvApplicationProfiler.cpp
  src/NvBuffer.cpp
  src/NvBufSurface.cpp
  # Remove the GUI rendering, simplifying dependencies
  # src/NvDrmRenderer.cpp
  # src/NvEglRenderer.cpp
  src/NvElement.cpp
  src/NvElementProfiler.cpp
  src/NvJpegDecoder.cpp
  src/NvJpegEncoder.cpp
  src/NvLogging.cpp
  src/NvUtils.cpp
  src/NvV4l2Element.cpp
  src/NvV4l2ElementPlane.cpp
  src/NvVideoDecoder.cpp
  src/NvVideoEncoder.cpp)
target_include_directories(jetson-multimedia-api SYSTEM PUBLIC include)
target_include_directories(jetson-multimedia-api SYSTEM PUBLIC include/libjpeg-8b)
target_link_libraries(jetson-multimedia-api PUBLIC ${NVMMAPI_LIBRARIES} pthread EGL)
target_link_libraries(jetson-multimedia-api PRIVATE /usr/lib/${CMAKE_LIBRARY_ARCHITECTURE}/tegra/libnvv4l2.so)
target_link_libraries(jetson-multimedia-api PRIVATE /usr/lib/${CMAKE_LIBRARY_ARCHITECTURE}/tegra/libnvbufsurface.so)
target_link_libraries(jetson-multimedia-api PRIVATE /usr/lib/${CMAKE_LIBRARY_ARCHITECTURE}/tegra/libnvbufsurftransform.so)
target_link_libraries(jetson-multimedia-api PUBLIC /usr/lib/${CMAKE_LIBRARY_ARCHITECTURE}/tegra/libnvjpeg.so)
target_compile_options(jetson-multimedia-api PRIVATE -Wno-error -Wno-all -Wno-extra -Wno-everything)
target_compile_definitions(jetson-multimedia-api PUBLIC TEGRA_ACCELERATE)
install(TARGETS jetson-multimedia-api DESTINATION lib)

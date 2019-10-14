# Author: Sudeep Pillai (Sep 14, 2014)
project (numpy-opencv-converter)
cmake_minimum_required (VERSION 2.6.0)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")
set( CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -Wl,--no-undefined" )

# Include package config, boost-python
find_package(PkgConfig REQUIRED)
find_package (Python COMPONENTS Interpreter Development NumPy)
FIND_PACKAGE(NumPy)
include_directories(${PYTHON_NUMPY_INCLUDE_DIR})
set(CMAKE_CXX_FLAGS   "${CMAKE_CXX_FLAGS} -fpermissive")
find_package(OpenCV 4 REQUIRED)
include(cmake/boost-python.cmake)


# configure opencv
#pkg_check_modules(OpenCV opencv)
message("Libs of OpenCV: " ${OpenCV_INCLUDE_DIRS} )
include_directories(${OpenCV_INCLUDE_DIRS})

# Build np<=>opencv converter library
boost_python_module(np_opencv_converter np_opencv_converter.cpp utils/conversion.cpp)
target_link_libraries(np_opencv_converter boost_system boost_python3 ${OpenCV_LIBS})

# Build test library
include_directories(${CMAKE_CURRENT_SOURCE_DIR})
boost_python_module(np_opencv_module tests/np_opencv_module.cpp)
#target_link_libraries(np_opencv_module boost_system boost_python3 np_opencv_converter ${OpenCV_LIBS} ${OpenCV_LDFLAGS})
target_link_libraries(np_opencv_module boost_system boost_python3 np_opencv_converter ${OpenCV_LIBS})

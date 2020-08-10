cmake_minimum_required(VERSION 3.1)
project(opencv_c_wrapper LANGUAGES C CXX)

find_package(OpenCV QUIET)
include_directories(.)
set(WRAPPER_SRCS Calibration.cpp)

set(CMAKE_BUILD_TYPE Release)

add_library(${PROJECT_NAME} ${WRAPPER_SRCS})

install(TARGETS ${PROJECT_NAME} EXPORT opencv_c_wrapper
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
)

file(GLOB_RECURSE HEADER_FILES RELATIVE ${CMAKE_SOURCE_DIR} *.h)
foreach(HEADER ${HEADER_FILES})
    string(REGEX MATCH "(.*)[/\\]" DIR ${HEADER})
    install(FILES ${HEADER} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME}/${DIR})
endforeach()

install(EXPORT opencv_c_wrapper
    DESTINATION share/opencv_c_wrapper/cmake
    FILE opencv_c_wrapper.cmake
)
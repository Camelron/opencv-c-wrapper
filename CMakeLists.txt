cmake_minimum_required(VERSION 3.1)
project(opencv-c-wrapper LANGUAGES C CXX)

find_package(OpenCV QUIET)
include_directories(.)
set(WRAPPER_SRCS Calibration.cpp)

set(CMAKE_BUILD_TYPE Release)

add_library(${PROJECT_NAME} ${WRAPPER_SRCS})

install(TARGETS ${PROJECT_NAME} EXPORT opencv-c-wrapper
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
)

file(GLOB_RECURSE HEADER_FILES RELATIVE ${CMAKE_SOURCE_DIR} *.h)
foreach(HEADER ${HEADER_FILES})
    string(REGEX MATCH "(.*)[/\\]" DIR ${HEADER})
    install(FILES ${HEADER} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME}/${DIR})
endforeach()

install(EXPORT opencv-c-wrapper
    DESTINATION share/opencv-c-wrapper/cmake
    FILE opencv-c-wrapper.cmake
)
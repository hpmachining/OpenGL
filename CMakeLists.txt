cmake_minimum_required(VERSION 3.16)

project(OpenGL)
set("${PROJECT_NAME}_VERSION" 1.0.0)

set(CMAKE_CXX_STANDARD 17)

add_subdirectory(OpenGL-Core "${PROJECT_SOURCE_DIR}/bin-int/OpenGL-Core")
add_subdirectory(OpenGL-Examples "${PROJECT_SOURCE_DIR}/bin-int/OpenGL-Examples")
add_subdirectory(OpenGL-Sandbox "${PROJECT_SOURCE_DIR}/bin-int/OpenGL-Sandbox")

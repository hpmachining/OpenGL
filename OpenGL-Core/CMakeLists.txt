cmake_minimum_required(VERSION 3.16)

project(OpenGL-Core)

set(CMAKE_CXX_STANDARD 17)

file(GLOB_RECURSE SOURCES
  src/*.h
  src/*.cpp
  vendor/stb_image/*.h
  vendor/stb_image/*.cpp
  vendor/Glad/*.h
  vendor/Glad/*.c
  vendor/glm/glm/*.inl
)

set(SOURCES
  ${SOURCES}
  vendor/imgui/imconfig.h
  vendor/imgui/imgui.h
  vendor/imgui/imgui.cpp
  vendor/imgui/imgui_draw.cpp
  vendor/imgui/imgui_internal.h
  vendor/imgui/imgui_widgets.cpp
  vendor/imgui/imstb_rectpack.h
  vendor/imgui/imstb_textedit.h
  vendor/imgui/imstb_truetype.h
  vendor/imgui/imgui_demo.cpp
)

include_directories(
  src
  vendor/spdlog/include
  vendor/glfw/include
  vendor/Glad/include
  vendor/imgui
  vendor/glm
  vendor/stb_image
)

add_subdirectory(vendor/glfw "${PROJECT_SOURCE_DIR}/../bin-int/glfw")

if(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
  add_compile_definitions(GLCORE_PLATFORM_LINUX)
  message(STATUS "Platform Linux")
elseif(${CMAKE_SYSTEM_NAME} MATCHES "Windows")
  add_compile_definitions(GLCORE_PLATFORM_WINDOWS)
  message(STATUS "Platform Windows")
endif()

add_library("${PROJECT_NAME}" STATIC
  ${SOURCES}
)

add_compile_definitions(
  GLFW_INCLUDE_NONE
)

if(${CMAKE_BUILD_TYPE} MATCHES "Debug")
	add_compile_definitions(
		GLCORE_DEBUG
	)
elseif(${CMAKE_BUILD_TYPE} MATCHES "Release")
	add_compile_definitions(
		GLCORE_RELEASE
	)
endif()

add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
	COMMAND ${CMAKE_COMMAND} -E copy
		${PROJECT_SOURCE_DIR}/../bin-int/glfw/src/libglfw3.a
		${PROJECT_SOURCE_DIR}/../bin/libglfw3.a
)

set_target_properties( "${PROJECT_NAME}"
	PROPERTIES
	ARCHIVE_OUTPUT_DIRECTORY "${PROJECT_SOURCE_DIR}/../bin"
)

cmake_minimum_required(VERSION 3.16)

project(OpenGL-Sandbox)

set(CMAKE_CXX_STANDARD 17)

file(GLOB_RECURSE SOURCES
	src/*.h
	src/*.cpp
)

include_directories(
	../OpenGL-Core/vendor/spdlog/include
	../OpenGL-Core/src
	../OpenGL-Core/vendor
	../OpenGL-Core/vendor/glm
	../OpenGL-Core/vendor/Glad/include
	../OpenGL-Core/vendor/imgui
)

add_compile_definitions(
	GLFW_INCLUDE_NONE
)

if(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
	add_compile_definitions(GLCORE_PLATFORM_LINUX)
	message(STATUS "Platform Linux")
elseif(${CMAKE_SYSTEM_NAME} MATCHES "Windows")
	add_compile_definitions(GLCORE_PLATFORM_WINDOWS)
	message(STATUS "Platform Windows")
endif()

add_executable(
	${PROJECT_NAME}
	${SOURCES}
) 

target_link_directories(${PROJECT_NAME} PUBLIC
	${PROJECT_SOURCE_DIR}/../bin
)

target_link_libraries(
	${PROJECT_NAME}
	OpenGL-Core
	glfw3
	pthread
	X11
	dl
)

set_target_properties( "${PROJECT_NAME}"
	PROPERTIES
	RUNTIME_OUTPUT_DIRECTORY "${PROJECT_SOURCE_DIR}/../bin"
)


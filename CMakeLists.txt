cmake_minimum_required (VERSION 3.8)
set(MY_FANCY_PROJECT_NAME "test")  # set your project name here

project (${MY_FANCY_PROJECT_NAME})


IF(WIN32)
	set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} /SUBSYSTEM:CONSOLE /ENTRY:mainCRTStartup")
	set(WIN32_BUILDINT_TYPES "x86") # If you want to build for 32 bit platform, change this to x86
	include_directories("${CMAKE_SOURCE_DIR}/extern/include")
	link_directories("${CMAKE_SOURCE_DIR}/extern/lib/${WIN32_BUILDINT_TYPES}")  # Where we put libs
	add_definitions(-DSDL_MAIN_HANDLED)  # Should add this or SDL2 while using its own provided WinMain
	set(GLEW_LIBRARY "glew32")
	set(OPENGL_LIBRARY "opengl32")
ELSEIF(UNIX)
	set(GLEW_LIBRARY "GLEW")
	set(OPENGL_LIBRARY "GL")
ENDIF()
find_package(SDL2)
find_package(GLEW)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/bin")
aux_source_directory("${CMAKE_SOURCE_DIR}/src" MANAGED_SOURCE)


IF(WIN32)

add_executable(${MY_FANCY_PROJECT_NAME} WIN32 ${MANAGED_SOURCE})

ELSE()
add_executable(${MANAGED_SOURCE})
ENDIF()

target_link_libraries(${MY_FANCY_PROJECT_NAME} SDL2 ${GLEW_LIBRARY} ${OPENGL_LIBRARY})
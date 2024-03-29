﻿cmake_minimum_required (VERSION 3.18)

add_executable (MainTest "src/Main.cpp")

target_include_directories (MainTest PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}/#in/Include")
target_include_directories (MainTest PRIVATE "${CMAKE_SOURCE_DIR}/#in/Include")
target_include_directories (MainTest PRIVATE "${CMAKE_BINARY_DIR}/#Headers")

target_link_libraries(MainTest
	PRIVATE "${CMAKE_SOURCE_DIR}/#in/Lib/${CMAKE_OS_NAME}/${CMAKE_PLATFORM_NAME}-${CMAKE_BUILD_TYPE}/CppLog.lib"
	PRIVATE "${CMAKE_SOURCE_DIR}/#in/Lib/${CMAKE_OS_NAME}/${CMAKE_PLATFORM_NAME}-${CMAKE_BUILD_TYPE}/glew32.lib"
	PRIVATE "${CMAKE_SOURCE_DIR}/#in/Lib/${CMAKE_OS_NAME}/${CMAKE_PLATFORM_NAME}-${CMAKE_BUILD_TYPE}/SDL2.lib"
	PRIVATE "${CMAKE_SOURCE_DIR}/#in/Lib/${CMAKE_OS_NAME}/${CMAKE_PLATFORM_NAME}-${CMAKE_BUILD_TYPE}/SDL2main.lib"
	PRIVATE "${CMAKE_SOURCE_DIR}/#in/Lib/${CMAKE_OS_NAME}/${CMAKE_PLATFORM_NAME}-${CMAKE_BUILD_TYPE}/SDL2test.lib"
	)


if(${CMAKE_OS_NAME} STREQUAL "Windows")
	add_custom_command(
		TARGET MainTest
		POST_BUILD
		COMMAND call "${CMAKE_CURRENT_SOURCE_DIR}/#buildsteps/CopyBinaries.bat"
					"${CMAKE_CURRENT_SOURCE_DIR}/#in/Bin/${CMAKE_OS_NAME}/${CMAKE_PLATFORM_NAME}-${CMAKE_BUILD_TYPE}"
					"${CMAKE_CURRENT_BINARY_DIR}"
					"${DYNAMIC_LIB_EXT}"
		COMMENT "Adds dynamic libraries to the same folder as the exe"
		VERBATIM
	)
	add_custom_command(
		TARGET MainTest
		POST_BUILD
		COMMAND call "${CMAKE_CURRENT_SOURCE_DIR}/#buildsteps/CopyBinaries.bat"
					"${CMAKE_SOURCE_DIR}/#in/Bin/${CMAKE_OS_NAME}/${CMAKE_PLATFORM_NAME}-${CMAKE_BUILD_TYPE}"
					"${CMAKE_CURRENT_BINARY_DIR}"
					"${DYNAMIC_LIB_EXT}"
		COMMENT "Adds dynamic libraries to the same folder as the exe"
		VERBATIM
	)
endif()

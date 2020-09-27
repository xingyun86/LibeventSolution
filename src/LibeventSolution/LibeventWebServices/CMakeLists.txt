# CMakeList.txt : CMake project for LibeventSolution, include source and define
# project specific logic here.
#
cmake_minimum_required (VERSION 3.8)

set(project_name LibeventWebServices)
project(${project_name})

include_directories(
	${CMAKE_CURRENT_SOURCE_DIR}
	${CMAKE_SOURCE_DIR}/vendor/libevent-2.1.12-stable/include
	${CMAKE_BINARY_DIR}/vendor/libevent-2.1.12-stable/include
	${CMAKE_SOURCE_DIR}/vendor/libevent-2.1.12-stable/WIN32-Code
)
link_directories(
	${CMAKE_CURRENT_BINARY_DIR}
	${CMAKE_BINARY_DIR}/vendor/libevent-2.1.12-stable/lib
)

FILE(GLOB SRCS "*.cpp" "*.c")
FILE(GLOB EXT_SRCS "${CMAKE_SOURCE_DIR}/vendor/libevent-2.1.12-stable/WIN32-Code/*.cpp" "${CMAKE_SOURCE_DIR}/vendor/libevent-2.1.12-stable/WIN32-Code/*.c")

# Add source to this project's executable.
add_executable (${project_name} ${SRCS} ${EXT_SRCS})

IF(MSVC)
	set(MSVC_LIBS wsock32 ws2_32)
ELSE(MSVC)
	set(MSVC_LIBS)
ENDIF(MSVC)
if (MSVC)
	list(APPEND project_name_cflags /W4)
	list(APPEND project_name_defines WIN32_LEAN_AND_MEAN _WIN32_WINNT=0x0501)
	MESSAGE(STATUS "CMAKE_BUILD_TYPE=" ${CMAKE_BUILD_TYPE})

	set(CompilerFlags
		CMAKE_CXX_FLAGS
		CMAKE_CXX_FLAGS_DEBUG
		CMAKE_CXX_FLAGS_RELEASE
		CMAKE_C_FLAGS
		CMAKE_C_FLAGS_DEBUG
		CMAKE_C_FLAGS_RELEASE
		)
	foreach(CompilerFlag ${CompilerFlags})
		string(REPLACE "/MD" "/MT" ${CompilerFlag} "${${CompilerFlag}}")
	endforeach()
	
	target_compile_definitions(${project_name}
							PRIVATE ${project_name_defines}
							)
	target_compile_options(${project_name}
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)

target_link_libraries(${project_name} 
	PRIVATE event
	PRIVATE event_core
	PRIVATE event_extra
	PRIVATE event_openssl
	PRIVATE ${MSVC_LIBS}
)
if(MSVC)
#	SET_TARGET_PROPERTIES(${project_name} PROPERTIES LINK_FLAGS ${ADMIN_LEVEL})
endif(MSVC)
# TODO: Add tests and install targets if needed.

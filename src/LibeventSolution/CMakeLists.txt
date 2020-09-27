# CMakeList.txt : Top-level CMake project file, do global configuration
# and include sub-projects here.
#
cmake_minimum_required (VERSION 3.8)

project ("LibeventSolution")

IF(MSVC)
set(ADMIN_LEVEL "/MANIFESTUAC:\"level='requireAdministrator' uiAccess='false'\"")
ENDIF(MSVC)

# Include sub-projects.
add_subdirectory ("vendor/libressl-3.1.1")
add_subdirectory ("vendor/libevent-2.1.12-stable")
add_subdirectory ("LibeventWebServices")
add_subdirectory ("LibeventSolution")

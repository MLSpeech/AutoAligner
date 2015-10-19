cmake_minimum_required(VERSION 2.8)

SET(START_DIR .)

set( CMAKE_RUNTIME_OUTPUT_DIRECTORY ${START_DIR}/builds/win32 )
set( CMAKE_LIBRARY_OUTPUT_DIRECTORY ${START_DIR}/builds/win32/lib )
set( CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${START_DIR}/builds/win32/lib )

project(cmd_line_project)

add_library(cmd_line STATIC ${START_DIR}/src/cmd_line.cpp)
target_include_directories (cmd_line PUBLIC ${START_DIR}/src/)

add_executable(tester ${START_DIR}/tester/tester.cpp)

target_link_libraries(tester LINK_PUBLIC cmd_line)

install(TARGETS tester DESTINATION bin)
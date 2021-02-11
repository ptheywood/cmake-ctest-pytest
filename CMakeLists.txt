cmake_minimum_required(VERSION VERSION 3.15 FATAL_ERROR)

project(cmake-ctest-pytest LANGUAGES NONE)


# Option to enable/disable tests.
option(BUILD_TESTS "Enable building tests" ON)

find_package(Python3 REQUIRED COMPONENTS Interpreter Development)
message(STATUS "Python found at " ${Python3_EXECUTABLE})

# get_filename_component(Python3_EXECUTABLE "${Python3_EXECUTABLE}" REALPATH)

message(STATUS "Executing python3 --version")
execute_process(COMMAND ${Python3_EXECUTABLE} --version)

message(STATUS "venv help?")
execute_process(COMMAND ${Python3_EXECUTABLE} -m venv -h)
message(STATUS "maybe created file?")

message(STATUS "Createing a venv using ${Python3_EXECUTABLE}")
execute_process(COMMAND ${Python3_EXECUTABLE} -m venv env)

message(STATUS "maybe created file?")
execute_process(COMMAND ls env)

# add_subdirectory(python)

# if(BUILD_TESTS)
#     add_subdirectory(tests)
# endif()
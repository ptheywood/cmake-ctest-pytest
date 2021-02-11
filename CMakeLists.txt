cmake_minimum_required(VERSION VERSION 3.15 FATAL_ERROR)

project(cmake-ctest-pytest LANGUAGES NONE)


# Option to enable/disable tests.
option(BUILD_TESTS "Enable building tests" ON)

find_package(Python3 REQUIRED COMPONENTS Interpreter Development)
message(STATUS "Python found at " ${Python3_EXECUTABLE})

get_filename_component(Python3_EXECUTABLE "${Python3_EXECUTABLE}" REALPATH)
message(STATUS "Python actually at " ${Python3_EXECUTABLE})


message(STATUS "Executing python3 --version")
execute_process(COMMAND ${Python3_EXECUTABLE} --version RESULT_VARIABLE ret)
if(ret EQUAL "1")
    message( FATAL_ERROR "Bad exit status")
endif()

message(STATUS "venv help?")
execute_process(COMMAND ${Python3_EXECUTABLE} -m venv -h RESULT_VARIABLE ret)
if(ret EQUAL "1")
    message( FATAL_ERROR "Bad exit status")
endif()

message(STATUS "Createing a venv using ${Python3_EXECUTABLE}")
execute_process(COMMAND ${Python3_EXECUTABLE} -m venv env RESULT_VARIABLE ret)
if(ret EQUAL "1")
    message( FATAL_ERROR "Bad exit status")
endif()

message(STATUS "List env")
execute_process(COMMAND ls env RESULT_VARIABLE ret)
if(ret EQUAL "1")
    message( FATAL_ERROR "Bad exit status")
endif()

# add_subdirectory(python)

# if(BUILD_TESTS)
#     add_subdirectory(tests)
# endif()
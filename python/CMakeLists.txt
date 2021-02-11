cmake_minimum_required(VERSION VERSION 3.15 FATAL_ERROR)

find_package(Python3 REQUIRED COMPONENTS Interpreter Development)
message(STATUS "Python found at " ${Python3_EXECUTABLE})



SET(PYTHON_MODULE_NAME mymodule)

add_custom_target("${PYTHON_MODULE_NAME}")

STRING(TOLOWER "${CMAKE_SYSTEM_NAME}" CMAKE_SYSTEM_NAME_LOWER)
SET(PYTHON_LIB_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib/${CMAKE_SYSTEM_NAME_LOWER}-x64/python)
SET(PYTHON_LIB_TEMP_DIRECTORY ${CMAKE_BINARY_DIR}/swig/python/${CMAKE_BUILD_TYPE})



# Function to find if python module MODULE_NAME is available, if not then install it to the Python user install directory.
# @todo - should not be installing packages into the users environment. For one --user might not even be accessible!
function(search_python_module MODULE_NAME)
  execute_process(
    COMMAND ${Python3_EXECUTABLE} -c "import ${MODULE_NAME}; print(${MODULE_NAME}.__version__) if hasattr(${MODULE_NAME}, '__version__') else print('Unknown');"
    RESULT_VARIABLE _RESULT
    OUTPUT_VARIABLE MODULE_VERSION
    ERROR_QUIET
    OUTPUT_STRIP_TRAILING_WHITESPACE
    )
  if(${_RESULT} STREQUAL "0")
    message(STATUS "Found python module: ${MODULE_NAME} (version \"${MODULE_VERSION}\")")
  else()
    message(WARNING "Can't find python module \"${MODULE_NAME}\", user install it using pip...")
    execute_process(
      COMMAND ${Python3_EXECUTABLE} -m pip install --upgrade --user ${MODULE_NAME}
      OUTPUT_STRIP_TRAILING_WHITESPACE
      )
  endif()
endfunction()

# Find setuptools and wheel which are both apparently required.
search_python_module(setuptools)
search_python_module(wheel)


#######################
## Python Packaging  ##
#######################
configure_file(
	setup.py.in
	${PYTHON_LIB_TEMP_DIRECTORY}/setup.py.in
	@ONLY)
file(GENERATE
	OUTPUT ${PYTHON_LIB_OUTPUT_DIRECTORY}/setup.py
	INPUT ${PYTHON_LIB_TEMP_DIRECTORY}/setup.py.in)

configure_file(
    __init__.py.in
    ${PYTHON_LIB_TEMP_DIRECTORY}/__init__.py.in
    @ONLY)

file(GENERATE
	OUTPUT ${PYTHON_LIB_OUTPUT_DIRECTORY}/${PYTHON_MODULE_NAME}/__init__.py
	INPUT ${PYTHON_LIB_TEMP_DIRECTORY}/__init__.py.in)
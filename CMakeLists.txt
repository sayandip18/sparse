cmake_minimum_required(VERSION 3.11.0)
project(sparse)
set(CMAKE_RC_COMPILER ${CMAKE_CXX_COMPILER})
include(FetchContent)

find_package(Python3 REQUIRED COMPONENTS Interpreter Development NumPy)
find_package(pybind11 CONFIG)
find_package(taco)

# Satisfy pybind11 dep from source if not found on system
if(NOT TARGET pybind11::pybind11)

  # To prevent download of the pybind11 source you can overwrite the following
  # behavior by setting the cmake variable FETCHCONTENT_SOURCE_DIR_PYBIND11 to a
  # local source directory
  FetchContent_Declare(
    pybind11
    GIT_REPOSITORY https://github.com/pybind/pybind11.git
    GIT_TAG        v2.6.2
  )
  FetchContent_MakeAvailable(pybind11)
endif()

if(NOT TARGET taco::taco)
  FetchContent_Declare(
    taco
    GIT_REPOSITORY https://github.com/leej3/taco.git
    GIT_TAG        cmake_changes
  )
  FetchContent_MakeAvailable(taco)
endif()

pybind11_add_module(_cxx sparse/_cxx.cxx)
install(TARGETS _cxx LIBRARY DESTINATION sparse)
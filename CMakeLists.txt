cmake_minimum_required (VERSION 3.21.2)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_BUILD_TYPE debug)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++2a")
set(CXX_FLAGS "-fconcepts-ts")

project (joystick-service)

find_package(cppzmq)
find_package(Threads REQUIRED)

include(ExternalProject)
set(EXTERNAL_INSTALL_LOCATION ${CMAKE_BINARY_DIR}/external)

ExternalProject_Add( argparse
    GIT_REPOSITORY https://github.com/p-ranav/argparse.git
    INSTALL_COMMAND cmake -E echo "Skipping install step."
  )
ExternalProject_Get_Property(argparse)

ExternalProject_Add(yaml
    GIT_REPOSITORY https://github.com/jbeder/yaml-cpp.git
    INSTALL_COMMAND cmake -E echo "Skipping install step."
)
ExternalProject_Get_Property(yaml)

include_directories(${EXTERNAL_INSTALL_LOCATION}/include)
set(INCLUDE_DIR ${PROJECT_SOURCE_DIR}/include)
include_directories(${INCLUDE_DIR})

add_executable ( ${CMAKE_PROJECT_NAME}
  src/joystick.cpp)

add_dependencies(${CMAKE_PROJECT_NAME}
  argparse
  yaml)

target_link_libraries(${CMAKE_PROJECT_NAME}
  ${EXTERNAL_INSTALL_LOCATION}/lib64/libyaml-cpp.a
  zmq )


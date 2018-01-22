cmake_minimum_required(VERSION 2.8.3)
project(dlib_catkin)

find_package(catkin_simple REQUIRED)
catkin_simple(ALL_DEPS_REQUIRED)

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

ExternalProject_Add(dlib_src
  GIT_REPOSITORY git@github.com:dorian3d/DLib.git
  GIT_TAG 70089a38056e8aebd5a2ebacbcb67d3751433f32
  CMAKE_ARGS -DCMAKE_INSTALL_PREFIX=${CATKIN_DEVEL_PREFIX}
)

cs_add_library(${PROJECT_NAME} src/dependency_tracker.cc)
add_dependencies(${PROJECT_NAME} dlib_src)
target_link_libraries(${PROJECT_NAME} 
  ${CATKIN_DEVEL_PREFIX}/lib/libDLib${CMAKE_SHARED_LIBRARY_SUFFIX})

cs_install()
cs_export(
  INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
  CFG_EXTRAS dlib-extras.cmake)

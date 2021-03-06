cmake_minimum_required(VERSION 2.8.3)
project(rosban_model_learning)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  rosban_bbo
)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -std=c++11")

###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if you package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
  INCLUDE_DIRS include
  LIBRARIES rosban_model_learning
  CATKIN_DEPENDS rosban_bbo
#  DEPENDS system_lib
)

###########
## Build ##
###########

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  )

# Directories containing sources files
set(DIRECTORIES
  src/rosban_model_learning/
  src/rosban_model_learning/models
  )

# Build ALL_SOURCES
set (SOURCES)
set (ALL_SOURCES)
set (PREFIXED_SOURCES)
foreach (DIRECTORY ${DIRECTORIES})
    include_directories("${CMAKE_CURRENT_SOURCE_DIR}/${DIRECTORY}")
    include (${DIRECTORY}/sources.cmake)

    foreach (SOURCE ${SOURCES})
        set (PREFIXED_SOURCES ${PREFIXED_SOURCES} ${DIRECTORY}/${SOURCE})
    endforeach (SOURCE)

    set (ALL_SOURCES ${ALL_SOURCES} ${PREFIXED_SOURCES})
endforeach (DIRECTORY)

# Declare a C++ library
add_library(rosban_model_learning
  ${ALL_SOURCES}
  )

target_link_libraries(rosban_model_learning
  ${catkin_LIBRARIES}
  )

# Binaries
add_executable(test_simple_angular_model src/tests/test_simple_angular_model.cpp)

target_link_libraries(test_simple_angular_model ${catkin_LIBRARIES} rosban_model_learning)
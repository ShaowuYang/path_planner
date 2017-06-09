cmake_minimum_required(VERSION 2.8.3)
project(path_planner)

find_package(catkin REQUIRED
    COMPONENTS
        cmake_modules
        roscpp
        pluginlib
        actionlib
        dynamic_reconfigure
        message_generation
        move_base_msgs
        nav_core
        tf
)
find_package(Eigen REQUIRED)
add_definitions(${EIGEN_DEFINITIONS})

# dynamic reconfigure
generate_dynamic_reconfigure_options(
    cfg/MoveBase.cfg
)

catkin_package(
    CATKIN_DEPENDS
        roscpp
        dynamic_reconfigure
)

include_directories(
    include
    ${catkin_INCLUDE_DIRS}
    ${EIGEN_INCLUDE_DIRS}
)

# move_base
add_library(path_planner
  src/path_planner.cpp
)
target_link_libraries(path_planner
    ${Boost_LIBRARIES}
    ${catkin_LIBRARIES}
    )
add_dependencies(path_planner move_base_gencfg)
add_dependencies(path_planner geometry_msgs_gencpp)

add_executable(path_planner_node
  src/path_planner_node.cpp
)
target_link_libraries(path_planner_node path_planner)
set_target_properties(path_planner_node PROPERTIES OUTPUT_NAME path_planner)
set_target_properties(path_planner_node PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/lib"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/lib"
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/bin")

#install(
#    TARGETS
#        path_planner
#        path_planner_node
#    ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#    LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#    RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
#)

### Mark cpp header files for installation
#install(DIRECTORY include/${PROJECT_NAME}/
#  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#  FILES_MATCHING PATTERN "*.h"
#)

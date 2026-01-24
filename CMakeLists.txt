cmake_minimum_required(VERSION 3.5)
project(gazebo_robotic_assets)

# Determine ROS version
if(DEFINED ENV{ROS_VERSION})
  set(ROS_VERSION $ENV{ROS_VERSION})
else()
  find_package(catkin QUIET)
  find_package(ament_cmake QUIET)
  if(catkin_FOUND AND NOT ament_cmake_FOUND)
    set(ROS_VERSION 1)
  elseif(ament_cmake_FOUND)
    set(ROS_VERSION 2)
  else()
    message(FATAL_ERROR "Neither catkin nor ament_cmake found. Please source a ROS workspace.")
  endif()
endif()

message(STATUS "Building for ROS ${ROS_VERSION}")

###########
## Build ##
###########

# Resolve for the package path at compile time.
configure_file(
  "${PROJECT_SOURCE_DIR}/include/${PROJECT_NAME}/package_path.h.in"
  "${PROJECT_SOURCE_DIR}/include/${PROJECT_NAME}/package_path.h" @ONLY
)

if(ROS_VERSION EQUAL 1)
  #################
  ## ROS1 Catkin ##
  #################
  find_package(catkin REQUIRED)

  catkin_package(
    INCLUDE_DIRS
      include
  )

  install(DIRECTORY include/${PROJECT_NAME}/
    DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  )

  install(DIRECTORY resources
    DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
  )

else()
  #################
  ## ROS2 Ament  ##
  #################
  find_package(ament_cmake REQUIRED)

  install(DIRECTORY include/
    DESTINATION include
  )

  install(DIRECTORY resources
    DESTINATION share/${PROJECT_NAME}
  )

  ament_export_include_directories(include)
  ament_package()

endif()

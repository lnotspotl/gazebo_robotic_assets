cmake_minimum_required(VERSION 3.8)
project(gazebo_robotic_assets)

find_package(ament_cmake REQUIRED)

###########
## Build ##
###########

# Create the include directory structure in the build directory
file(MAKE_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/include/${PROJECT_NAME})

# Resolve for the package path at compile time.
configure_file (
  "${PROJECT_SOURCE_DIR}/include/${PROJECT_NAME}/package_path.h.in"
  "${CMAKE_CURRENT_BINARY_DIR}/include/${PROJECT_NAME}/package_path.h" @ONLY
)

#############
## Install ##
#############

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION include/${PROJECT_NAME}
)

# Install the generated header file
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/include/${PROJECT_NAME}/package_path.h
  DESTINATION include/${PROJECT_NAME}
)

install(DIRECTORY resources
  DESTINATION share/${PROJECT_NAME}
)

# Export include directories
ament_export_include_directories(include)

ament_package()
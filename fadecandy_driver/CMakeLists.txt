cmake_minimum_required(VERSION 3.5)
project(fadecandy_driver)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()
add_compile_options(-Wall)

##################
##  Build setup ##
##################

find_package(ament_cmake)
find_package(diagnostic_updater REQUIRED)
find_package(fadecandy_msgs REQUIRED)
find_package(rclcpp REQUIRED)

find_package(PkgConfig REQUIRED)
pkg_check_modules(libusb libusb-1.0)

ament_export_dependencies(libusb, diagnostic_updater, fadecandy_msgs)

##############
##   Build  ##
##############

include_directories(
  ${INCLUDE_DIRS}
  ${libusb_INCLUDE_DIRS}
)

add_library(${PROJECT_NAME}
  src/${PROJECT_NAME}.cpp
  src/util.cpp
)
ament_target_dependencies(${PROJECT_NAME} libusb)

add_library(${PROJECT_NAME}_ros
  src/ros/${PROJECT_NAME}_ros.cpp
)
target_link_libraries(${PROJECT_NAME}_ros ${PROJECT_NAME})
ament_target_dependencies(${PROJECT_NAME}_ros fadecandy_msgs diagnostic_updater )

add_executable(fadecandy_node src/ros/${PROJECT_NAME}_ros_node.cpp)
target_link_libraries(fadecandy_node ${PROJECT_NAME}_ros)

if (BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()

#############
## Install ##
#############

install(
  TARGETS
    ${PROJECT_NAME}
    ${PROJECT_NAME}_ros
    fadecandy_node
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

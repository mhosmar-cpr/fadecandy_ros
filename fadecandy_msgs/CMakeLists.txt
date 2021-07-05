cmake_minimum_required(VERSION 3.5)
project(fadecandy_msgs)

set(CMAKE_CXX_STANDARD 14)

find_package(rosidl_default_generators REQUIRED)

find_package(std_msgs REQUIRED)

ament_export_dependencies(rosidl_default_runtime std_msgs)

rosidl_generate_interfaces(${PROJECT_NAME}
  "msg/LEDArray.msg"
  "msg/LEDStrip.msg"
  DEPENDENCIES std_msgs
)

ament_package()

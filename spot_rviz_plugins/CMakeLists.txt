cmake_minimum_required(VERSION 3.22)

project(spot_rviz_plugins)

find_package(ament_cmake REQUIRED)
find_package(ament_cmake_ros REQUIRED)
find_package(std_msgs REQUIRED)
find_package(pluginlib REQUIRED)
find_package(std_srvs REQUIRED)
find_package(spot_msgs REQUIRED)
find_package(rviz_common REQUIRED)
find_package(Qt6 COMPONENTS UiTools REQUIRED)

set(CMAKE_AUTOMOC ON)

qt6_wrap_cpp(MOC_FILES "include/spot_rviz_plugins/spot_panel.hpp")

add_library(
  spot_rviz_plugins
  "src/spot_panel.cpp"
  ${MOC_FILES}
  )

install(FILES rviz_common_plugins.xml DESTINATION share/${PROJECT_NAME})

target_include_directories(spot_rviz_plugins PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)

target_link_libraries(spot_rviz_plugins PUBLIC 
  Qt6::UiTools
  pluginlib::pluginlib
  rclcpp::rclcpp
  rviz_common::rviz_common
  spot_msgs::spot_msgs
  std_msgs::std_msgs
  std_srvs::std_srvs
)

install(
  TARGETS spot_rviz_plugins
  EXPORT export_spot_rviz_plugins
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  RUNTIME DESTINATION bin
)

install(DIRECTORY include/
        DESTINATION include
)
install(FILES rviz_common_plugins.xml
        DESTINATION share/${PROJECT_NAME}
)
install(DIRECTORY resource DESTINATION share/${PROJECT_NAME})

ament_export_include_directories(include)
ament_export_targets(export_spot_rviz_plugins)
pluginlib_export_plugin_description_file(rviz_common rviz_common_plugins.xml)

ament_package()

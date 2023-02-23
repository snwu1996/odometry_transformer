cmake_minimum_required(VERSION 2.8.4)
project(odometry_transformer)

find_package(catkin
  REQUIRED COMPONENTS
  dynamic_reconfigure
  geometry_msgs
  nav_msgs
  nodelet
  pluginlib
  roscpp
  tf2_geometry_msgs
  tf2_ros
  )

include_directories(include ${catkin_INCLUDE_DIRS})

#######################
# DYNAMIC RECONFIGURE #
#######################
generate_dynamic_reconfigure_options(cfg/OdometryTransformer.cfg)

catkin_package()

#############
# LIBRARIES #
#############
add_library(${PROJECT_NAME} src/${PROJECT_NAME}.cc
                            src/${PROJECT_NAME}_nodelet.cc)
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES})

############
# Binaries #
############
add_executable(${PROJECT_NAME}_node src/${PROJECT_NAME}_node.cc)
target_link_libraries(${PROJECT_NAME}_node ${PROJECT_NAME} ${catkin_LIBRARIES})
add_dependencies(${PROJECT_NAME}_node ${PROJECT_NAME}_gencfg)

add_executable(test_odometry_publisher test/test_odometry_publisher.cc)
target_link_libraries(test_odometry_publisher ${catkin_LIBRARIES})

##########
# EXPORT #
##########
install(TARGETS ${PROJECT_NAME} ${PROJECT_NAME}_node
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
install(FILES nodelet_plugin.xml
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)

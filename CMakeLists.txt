cmake_minimum_required(VERSION 2.8.3)
project(youbot_driver_ros_interface)


find_package(catkin REQUIRED COMPONENTS youbot_driver roscpp roslib rosconsole actionlib tf control_msgs brics_actuator diagnostic_msgs geometry_msgs nav_msgs pr2_msgs std_msgs std_srvs trajectory_msgs)
find_package(Boost REQUIRED COMPONENTS thread date_time filesystem system)

catkin_package(
	CATKIN_DEPENDS youbot_driver rosconsole roscpp pr2_msgs brics_actuator tf control_msgs diagnostic_msgs geometry_msgs nav_msgs pr2_msgs std_msgs std_srvs trajectory_msgs
)

## preprocessor definitions
add_definitions(-DYOUBOT_CONFIGURATIONS_DIR=${youbot_driver_CONFIG_DIR})

## Set Compiler flags
include_directories(
	include
	${catkin_INCLUDE_DIRS}
	${Boost_INCLUDE_DIR}
)	


add_executable(youbot_driver_ros_interface 
    src/youbot_oodl.cpp 
    src/YouBotOODLWrapper.cpp
    src/YouBotConfiguration.cpp
)

target_link_libraries(youbot_driver_ros_interface ${catkin_LIBRARIES} ${Boost_LIBRARIES})

add_dependencies(youbot_driver_ros_interface ${catkin_EXPORTED_TARGETS})

install(DIRECTORY launch/
	DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch
)

#test programs
add_executable(youbot_arm_test 
    examples/youbot_arm_test.cpp 
)
target_link_libraries(youbot_arm_test ${catkin_LIBRARIES} ${Boost_LIBRARIES} ${youbot_driver_LIBRARIES})

add_executable(youbot_2nd_arm_test 
	examples/youbot_2nd_arm_test.cpp
)
target_link_libraries(youbot_2nd_arm_test ${catkin_LIBRARIES} ${Boost_LIBRARIES} ${youbot_driver_LIBRARIES})

install(TARGETS youbot_driver_ros_interface youbot_arm_test
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(PROGRAMS src/examples/youbot_keyboard_teleop.py
         DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

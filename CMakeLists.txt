cmake_minimum_required(VERSION 2.8.3)
project(youbot_driver_ros_interface)


find_package(catkin REQUIRED COMPONENTS message_generation std_msgs roscpp actionlib tf control_msgs brics_actuator pr2_msgs arm_kinematics)
find_package(Boost REQUIRED COMPONENTS thread date_time filesystem system)
find_package(youbot_driver REQUIRED)

catkin_package(
	CATKIN_DEPENDS pr2_msgs message_runtime brics_actuator
)

## preprocessor definitions
ADD_DEFINITIONS(-DYOUBOT_CONFIGURATIONS_DIR=${youbot_driver_CONFIG_DIR})

## Set Compiler flags
INCLUDE_DIRECTORIES(
	include
	${catkin_INCLUDE_DIRS}
	${Boost_INCLUDE_DIR}
	${youbot_driver_INCLUDE_DIRS}
)	


add_executable(youbot_driver_ros_interface 
    src/youbot_oodl.cpp 
    src/YouBotOODLWrapper.cpp
    src/YouBotConfiguration.cpp
)

target_link_libraries(youbot_driver_ros_interface ${catkin_LIBRARIES} ${Boost_LIBRARIES} ${youbot_driver_LIBRARIES})

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

#add_executable(youbot_arm_velocity_test 
#    examples/youbot_arm_velocity_test.cpp 
#)

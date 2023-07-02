cmake_minimum_required(VERSION 2.4.6)
project(dso_ros)

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 NEW)
endif(COMMAND cmake_policy)

set(CMAKE_CXX_FLAGS "-O3 -g -march=native -std=c++17 -Wall")
set(CMAKE_CXX_STANDARD 17)

include(GNUInstallDirs)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  cv_bridge
  sensor_msgs
)

find_package(Pangolin 0.2 REQUIRED)
message("\n\n---- FOUND Pangolin Headers at\n\"${Pangolin_INCLUDE_DIRS}\"")
message("---- FOUND Pangolin Libs at\n\"${Pangolin_LIBRARIES}\"")

find_package(OpenCV REQUIRED)
message("\n\n---- FOUND OpenCV Headers at\n\"${OpenCV_INCLUDE_DIRS}\"")
message("---- FOUND OpenCV Libs at\n\"${OpenCV_LIBS}\"")

find_package(Boost COMPONENTS system thread filesystem REQUIRED) 
message("\n\n---- FOUND Boost Headers at\n\"${Boost_INCLUDE_DIRS}\"")
message("---- FOUND Boost Libs at\n\"${Boost_LIBRARIES}\"")

find_package(Eigen3 REQUIRED)

set(DSO_INCLUDE_DIRS /usr/local/include/dso;/usr/local/include/dso/thirdparty/Sophus)
set(DSO_LIBRARIES /usr/local/lib/libdso.a)

catkin_package()

set(SOURCE_FILES         
  src/main.cpp
)

include_directories(
  ${catkin_INCLUDE_DIRS}
  ${Boost_INCLUDE_DIRS}
  ${PROJECT_SOURCE_DIR}/src
  ${DSO_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
  ${Pangolin_INCLUDE_DIRS}
  ${EIGEN3_INCLUDE_DIR}
)  

add_executable(dso_live src/main.cpp ${SOURCE_FILES})
target_link_libraries(dso_live 
  ${catkin_LIBRARIES}
  ${Boost_LIBRARIES}
  ${DSO_LIBRARIES}
  ${Pangolin_LIBRARIES} 
  ${OpenCV_LIBS}
  boost_thread
)
# rosbuild_link_boost(dso_live thread)

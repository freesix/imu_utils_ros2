cmake_minimum_required(VERSION 3.8)
project(imu_utils)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(cv_bridge REQUIRED)
find_package(message_filters REQUIRED)
find_package(code_utils REQUIRED)

find_package(OpenCV REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(Ceres REQUIRED)
find_package(Boost REQUIRED)
find_package(VTK REQUIRED)
find_package(Iconv REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS}
                    ${EIGEN3_INCLUDE_DIR}
                    ${CERES_INCLUDE_DIRS}
                    ${Boost_INCLUDE_DIRS}
                    "../code_utils/include")


set(ACC_LIB_SOURCE_FILES
    ${PROJECT_SOURCE_DIR}/src/acc_lib/allan_acc.cpp
    ${PROJECT_SOURCE_DIR}/src/acc_lib/fitallan_acc.cpp
    )
set(GYR_LIB_SOURCE_FILES
    ${PROJECT_SOURCE_DIR}/src/gyr_lib/allan_gyr.cpp
    ${PROJECT_SOURCE_DIR}/src/gyr_lib/fitallan_gyr.cpp
    )

add_executable(imu_an
    src/imu_an.cpp
    ${GYR_LIB_SOURCE_FILES}
    ${ACC_LIB_SOURCE_FILES}
    )
target_link_libraries(imu_an ${OpenCV_LIBS} ${CERES_LIBRARIES})
ament_target_dependencies(imu_an rclcpp sensor_msgs geometry_msgs code_utils cv_bridge message_filters)

install(
    DIRECTORY launch
    DESTINATION share/${PROJECT_NAME}
    )

install(
    TARGETS imu_an
    DESTINATION lib/${PROJECT_NAME}
    )
    
ament_package()

cmake_minimum_required(VERSION 2.8.3)
project(detected_objects_visualizer)

find_package(catkin REQUIRED COMPONENTS
        roscpp
        rockauto_msgs
        geometry_msgs
        sensor_msgs
        tf
        visualization_msgs
        )

catkin_package(
        CATKIN_DEPENDS
        roscpp
        rockauto_msgs
        geometry_msgs
        sensor_msgs
        tf
        visualization_msgs
)

find_package(Eigen3 QUIET)

if (NOT EIGEN3_FOUND)
    find_package(cmake_modules REQUIRED)
    find_package(Eigen REQUIRED)
    set(EIGEN3_INCLUDE_DIRS ${EIGEN_INCLUDE_DIRS})
    set(EIGEN3_LIBRARIES ${EIGEN_LIBRARIES})
else ()
    set(EIGEN3_INCLUDE_DIRS ${EIGEN3_INCLUDE_DIR})
endif ()

#Rviz Marker visualization
add_executable(visualize_detected_objects
        include/visualize_detected_objects.h
        src/visualize_detected_objects_main.cpp
        src/visualize_detected_objects.cpp
        )

target_include_directories(visualize_detected_objects PRIVATE
        ${OpenCV_INCLUDE_DIR}
        ${catkin_INCLUDE_DIRS}
        ${Boost_INCLUDE_DIRS}
        ${EIGEN3_INCLUDE_DIRS}
        include
        )

target_link_libraries(visualize_detected_objects
        ${OpenCV_LIBRARIES}
        ${EIGEN3_LIBRARIES}
        ${catkin_LIBRARIES}
        )

add_dependencies(visualize_detected_objects
        ${catkin_EXPORTED_TARGETS}
        )

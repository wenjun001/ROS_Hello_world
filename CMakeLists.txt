# %Tag(FULLTEXT)%
cmake_minimum_required(VERSION 2.8.3)
project(beginner_tutorials)



## Find catkin and any catkin packages
find_package(catkin REQUIRED COMPONENTS roscpp rospy std_msgs genmsg)
find_package(fastcdr REQUIRED)
find_package(fastrtps REQUIRED)
find_package(OpenCV REQUIRED)

include(CheckCXXCompilerFlag)
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_COMPILER_IS_CLANG OR
        CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    check_cxx_compiler_flag(-std=c++11 SUPPORTS_CXX11)
    if(SUPPORTS_CXX11)
        add_compile_options(-std=c++11)
    else()
        message(FATAL_ERROR "Compiler doesn't support C++11")
    endif()
endif()


include_directories(
        ${catkin_INCLUDE_DIRS}
        ${OpenCV_INCLUDE_DIRS}
)


## Declare ROS messages and services
add_message_files(FILES Num.msg)
add_service_files(FILES AddTwoInts.srv)

## Generate added messages and services
generate_messages(DEPENDENCIES std_msgs)

## Declare a catkin package
catkin_package()


file(GLOB HELLOWORLD_EXAMPLE_SOURCES_CXX "src/hello/*.cxx")
file(GLOB HELLOWORLD_EXAMPLE_SOURCES_CPP "src/hello/*.cpp")

file(GLOB HELLOWORLD_EXAMPLE_SOURCES_CXX2 "src/flow/*.cxx")
file(GLOB HELLOWORLD_EXAMPLE_SOURCES_CPP2 "src/flow/*.cpp")



## Build talker and listener
include_directories(include ${catkin_INCLUDE_DIRS})

add_executable(talker src/talker.cpp  ${HELLOWORLD_EXAMPLE_SOURCES_CXX} ${HELLOWORLD_EXAMPLE_SOURCES_CPP}  ${HELLOWORLD_EXAMPLE_SOURCES_CXX2} ${HELLOWORLD_EXAMPLE_SOURCES_CPP2})
target_link_libraries(talker ${catkin_LIBRARIES}  fastrtps fastcdr ${OpenCV_LIBS} )
add_dependencies(talker beginner_tutorials_generate_messages_cpp)

add_executable(listener src/listener.cpp ${HELLOWORLD_EXAMPLE_SOURCES_CXX} ${HELLOWORLD_EXAMPLE_SOURCES_CPP})
target_link_libraries(listener ${catkin_LIBRARIES} fastrtps fastcdr  ${OpenCV_LIBS} )
add_dependencies(listener beginner_tutorials_generate_messages_cpp)

# %EndTag(FULLTEXT)%

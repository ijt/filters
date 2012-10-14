# http://ros.org/doc/groovy/api/catkin/html/user_guide/supposed.html
cmake_minimum_required(VERSION 2.8.3)
project(filters)
find_package(catkin REQUIRED COMPONENTS rostest)

catkin_package(
   INCLUDE_DIRS include
   LIBRARIES ${PROJECT_NAME})

#Plugins
add_library(mean src/mean.cpp)
add_library(params src/test_params.cpp)
add_library(increment src/increment.cpp)
add_library(median src/median.cpp)
add_library(transfer_function src/transfer_function.cpp)

# Test median filter
add_executable(median_test EXCLUDE_FROM_ALL test/test_median.cpp )
target_link_libraries(median_test median) 
add_gtest_build_flags(median_test)
add_rostest(test/test_median.launch)

#test transfer function filter
add_executable(transfer_function_test EXCLUDE_FROM_ALL test/test_transfer_function.cpp)
target_link_libraries(transfer_function_test transfer_function)
add_gtest_build_flags(transfer_function_test)
add_rostest(test/test_transfer_function.launch)

#test mean filter
add_executable(mean_test EXCLUDE_FROM_ALL test/test_mean.cpp)
target_link_libraries(mean_test mean)
add_gtest_build_flags(mean_test)
add_rostest(test/test_mean.launch)

#test params filter
add_executable(params_test EXCLUDE_FROM_ALL test/test_params.cpp)
target_link_libraries(params_test params)
add_gtest_build_flags(params_test)
add_rostest(test/test_params.launch)

#Test plugin loading into filter chain
add_executable(chain_test EXCLUDE_FROM_ALL test/test_chain.cpp)
add_gtest_build_flags(chain_test)
add_rostest(test/test_chain.launch)
find_package(Boost REQUIRED COMPONENTS system)
include_directories(${Boost_INCLUDE_DIRS})
target_link_libraries(chain_test ${Boost_LIBRARIES})

# Test reltime safe buffer class
add_gtest(realtime_buffer_test EXCLUDE_FROM_ALL test/test_realtime_circular_buffer.cpp)

cmake_minimum_required(VERSION 3.6.0)

# Extract package name and version
find_package(ros_industrial_cmake_boilerplate REQUIRED)
extract_package_metadata(pkg)
project(${pkg_extracted_name} VERSION ${pkg_extracted_version} LANGUAGES CXX)

find_package(Eigen3 REQUIRED)
find_package(console_bridge REQUIRED)

## Declare a C++ library
add_library(${PROJECT_NAME}
   src/stomp.cpp
   src/utils.cpp
)
target_link_libraries(${PROJECT_NAME}
  Eigen3::Eigen
  console_bridge::console_bridge
  )
target_cxx_version(${PROJECT_NAME} PUBLIC VERSION 11)
target_include_directories(${PROJECT_NAME} PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
                                                  "$<INSTALL_INTERFACE:include>")

add_executable(${PROJECT_NAME}_example examples/stomp_example.cpp)
target_link_libraries(${PROJECT_NAME}_example ${PROJECT_NAME})
target_cxx_version(${PROJECT_NAME}_example PUBLIC VERSION 11)
target_include_directories(${PROJECT_NAME} PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/examples>"
                                                  "$<INSTALL_INTERFACE:examples>")

configure_package(NAMESPACE stomp TARGETS ${PROJECT_NAME} ${PROJECT_NAME}_example)

## Mark cpp header files for installation
install(
  DIRECTORY include/${PROJECT_NAME}
  DESTINATION include
  FILES_MATCHING
  PATTERN "*.h"
  PATTERN ".svn" EXCLUDE)

if(STOMP_ENABLE_TESTING)
  enable_testing()
  add_run_tests_target(ENABLE ${STOMP_ENABLE_RUN_TESTING})
  add_subdirectory(test)
endif()

cmake_minimum_required(VERSION 3.6.0)

# Extract package name and version
find_package(ros_industrial_cmake_boilerplate REQUIRED)
extract_package_metadata(pkg)
project(${pkg_extracted_name} VERSION ${pkg_extracted_version} LANGUAGES CXX)

if(NOT DEFINED BUILD_SHARED_LIBS)
  set(BUILD_SHARED_LIBS ON)
endif()

if(NOT DEFINED STOMP_ENABLE_TESTING)
  set(STOMP_ENABLE_TESTING OFF)
endif()

find_package(Eigen3 REQUIRED)
find_package(console_bridge REQUIRED)
if(NOT TARGET console_bridge::console_bridge)
  add_library(console_bridge::console_bridge INTERFACE IMPORTED)
  set_target_properties(console_bridge::console_bridge PROPERTIES INTERFACE_INCLUDE_DIRECTORIES
                                                                  ${console_bridge_INCLUDE_DIRS})
  set_target_properties(console_bridge::console_bridge PROPERTIES INTERFACE_LINK_LIBRARIES ${console_bridge_LIBRARIES})
else()
  get_target_property(CHECK_INCLUDE_DIRECTORIES console_bridge::console_bridge INTERFACE_INCLUDE_DIRECTORIES)
  if(NOT ${CHECK_INCLUDE_DIRECTORIES})
    set_target_properties(console_bridge::console_bridge PROPERTIES INTERFACE_INCLUDE_DIRECTORIES
                                                                    ${console_bridge_INCLUDE_DIRS})
  endif()
endif()

# Declare a C++ library
add_library(${PROJECT_NAME} src/stomp.cpp src/utils.cpp)
target_link_libraries(${PROJECT_NAME} Eigen3::Eigen console_bridge::console_bridge)
target_cxx_version(${PROJECT_NAME} PUBLIC VERSION 11)
target_include_directories(${PROJECT_NAME} PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
                                                  "$<INSTALL_INTERFACE:include>")

add_executable(${PROJECT_NAME}_example examples/stomp_example.cpp)
target_link_libraries(${PROJECT_NAME}_example ${PROJECT_NAME})
target_cxx_version(${PROJECT_NAME}_example PUBLIC VERSION 11)
target_include_directories(${PROJECT_NAME}_example PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/examples>"
                                                          "$<INSTALL_INTERFACE:examples>")

configure_package(NAMESPACE stomp TARGETS ${PROJECT_NAME} ${PROJECT_NAME}_example)

# Mark cpp header files for installation
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

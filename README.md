# stomp_ros

[![license - apache 2.0](https://img.shields.io/:license-Apache%202.0-yellowgreen.svg)](https://opensource.org/licenses/Apache-2.0)

## Build Status

Platform             | CI Status
---------------------|:---------
Linux (Focal)        |[![Build Status](https://github.com/ros-industrial/stomp/actions/workflows/focal_build.yml/badge.svg?branch=main)](https://github.com/ros-industrial/stomp/actions/workflows/focal_build.yml)
Linux (Bionic)       |[![Build Status](https://github.com/ros-industrial/stomp/actions/workflows/bionic_build.yml/badge.svg?branch=main)](https://github.com/ros-industrial/stomp/actions/workflows/bionic_build.yml)
Windows              |[![Build Status](https://github.com/ros-industrial/stomp/actions/workflows/windows_noetic_build.yml/badge.svg?branch=main)](https://github.com/ros-industrial/stomp/actions/workflows/windows_noetic_build.yml)
Lint  (Clang-Format) |[![Build Status](https://github.com/ros-industrial/stomp/actions/workflows/clang_format.yml/badge.svg?branch=main)](https://github.com/ros-industrial/stomp/actions/workflows/clang_format.yml)
Lint  (CMake-Format) |[![Build Status](https://github.com/ros-industrial/stomp/actions/workflows/cmake_format.yml/badge.svg?branch=main)](https://github.com/ros-industrial/stomp/actions/workflows/cmake_format.yml)

#### Build
- Build the workspace:
  - Cd into the catkin workspace directory and type the following command:
```
catkin build
```

#### Build Unit Test
Cd into the catkin workspace directory and type the following command:
```
catkin build --cmake-args -DSTOMP_ENABLE_TESTING=ON 
```

#### Run Unit Test
Cd into the catkin workspace stomp build directory and type the following command:
```
ctest
```

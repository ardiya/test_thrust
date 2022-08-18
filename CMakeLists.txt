cmake_minimum_required(VERSION 3.16)
if (CMAKE_VERSION VERSION_GREATER_EQUAL 3.18)
  cmake_policy(SET CMP0104 OLD)
endif()

project(test_thrust CXX)

set(THRUST_REQUIRED_SYSTEMS CPP)
set(THRUST_OPTIONAL_SYSTEMS CUDA TBB OMP)

add_subdirectory(third_party/thrust)

find_package(Thrust REQUIRED CONFIG)
thrust_create_target(ThrustCPP HOST CPP DEVICE CPP)

include_directories(include)

add_executable(helloCPP src/main_hello.cpp)
target_link_libraries(helloCPP ThrustCPP)

if (THRUST_OMP_FOUND)
  thrust_create_target(ThrustOMP HOST CPP DEVICE OMP)
  add_executable(helloOMP src/main_hello.cpp)
  target_link_libraries(helloOMP ThrustOMP)
endif()

if (THRUST_TBB_FOUND)
  thrust_create_target(ThrustTBB HOST CPP DEVICE TBB)
  add_executable(helloTBB src/main_hello.cpp)
  target_link_libraries(helloTBB ThrustTBB)
endif()

if (THRUST_CUDA_FOUND)
  enable_language(CUDA)
  thrust_create_target(ThrustCUDA HOST CPP DEVICE CUDA)
  add_executable(helloCUDA src/main_hello.cpp)
  target_link_libraries(helloCUDA ThrustCUDA)
endif()

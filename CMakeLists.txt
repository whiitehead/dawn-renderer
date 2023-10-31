cmake_minimum_required(VERSION 3.13) # CMake version check
project(app)                         # Create project "app"
set(CMAKE_CXX_STANDARD 20)           # Enable C++20 standard

option(BUILD_SHARED_LIBS "Build using shared libraries" ON)
set(BUILD_SHARED_LIBS, OFF)

add_executable(app "src/main.cpp")

if(EMSCRIPTEN)
  set_target_properties(app PROPERTIES SUFFIX ".html")
  target_link_options(app PRIVATE "-sUSE_WEBGPU=1" "-sUSE_GLFW=3")
else()
  set(DAWN_FETCH_DEPENDENCIES ON)
  add_subdirectory("dawn" EXCLUDE_FROM_ALL)
  target_link_libraries(app PRIVATE webgpu_cpp webgpu_dawn webgpu_glfw)
endif()
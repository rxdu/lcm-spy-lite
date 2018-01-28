# Include path
include_directories(${CMAKE_CURRENT_SOURCE_DIR})

add_executable(lcm-spy-lite main.c)
target_link_libraries(lcm-spy-lite spylite)
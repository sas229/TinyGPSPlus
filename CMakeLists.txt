# TinyGPSPlus interface library build.
add_library(TinyGPSPlus INTERFACE)
target_sources(TinyGPSPlus INTERFACE ${CMAKE_CURRENT_LIST_DIR}/src/TinyGPSPlus.cpp)
target_include_directories(TinyGPSPlus INTERFACE ${CMAKE_CURRENT_LIST_DIR}/src/)
target_link_libraries(TinyGPSPlus INTERFACE pico_stdlib)
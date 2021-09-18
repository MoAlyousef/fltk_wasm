cmake_minimum_required(VERSION 3.15)
project(fltk_wasm)

set(FLTK_BUILD_TEST OFF CACHE BOOL " " FORCE)
add_subdirectory(fltk)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DEMSCRIPTEN -s WASM=1 -s USE_SDL=2 -s USE_FREETYPE=1")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DEMSCRIPTEN -s WASM=1 -s USE_SDL=2 -s USE_FREETYPE=1")

add_executable(main src/main.cpp)
target_include_directories(main PRIVATE ${CMAKE_BINARY_DIR}/FL ${CMAKE_CURRENT_LIST_DIR}/fltk/FL)
target_link_libraries(main PRIVATE fltk)
target_link_options(main PRIVATE "--shell-file shell_minimal.html")
set_target_properties(main PROPERTIES SUFFIX .html)
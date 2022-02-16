cmake_minimum_required(VERSION 3.14)
project(bongocat-xinput)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_FLAGS -D_HAS_AUTO_PTR_ETC=1)
option(CMAKE_USE_WIN32_THREADS_INIT "using WIN32 threads" ON)
set (CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})
set (CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})

include(FetchContent)
FetchContent_Declare(
  sfml
  GIT_REPOSITORY https://github.com/SFML/SFML.git
  GIT_TAG        2.5.1
)
FetchContent_Declare(
  freetype
  URL      https://download.savannah.gnu.org/releases/freetype/freetype-2.11.1.tar.gz
)

FetchContent_MakeAvailable(sfml freetype)

file(GLOB_RECURSE APP_SOURCES
    "./src/*.h"
    "./src/*.cpp"
    "./"
)

add_compile_definitions(_HAS_AUTO_PTR_ETC=1)

add_executable(bongocat-xinput WIN32
    ${APP_SOURCES}
)

target_include_directories(bongocat-xinput PUBLIC ${sfml_SOURCE_DIR}/include)

target_include_directories(bongocat-xinput PUBLIC
    "./include"
)

link_directories(${sfml_BINARY_DIR})
message(${sfml_BINARY_DIR}/lib/Debug)

find_package(freetype)

target_link_libraries(
    bongocat-xinput
    PRIVATE
    sfml-graphics
    sfml-window
    sfml-system
    opengl32
    freetype
    winmm
    gdi32
)
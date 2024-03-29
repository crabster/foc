# minimum required CMAKE version
CMAKE_MINIMUM_REQUIRED(VERSION 3.7 FATAL_ERROR)

project(foc)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

# compiler must be 11 or 14
set(CMAKE_CXX_STANDARD 17)

# required if linking to static library
add_definitions(-DANTLR4CPP_STATIC)

# using /MD flag for antlr4_runtime (for Visual C++ compilers only)
set(ANTLR4_WITH_STATIC_CRT OFF)
# add external build for antlrcpp
include(ExternalAntlr4Cpp)
# add antrl4cpp artifacts to project environment
include_directories(${ANTLR4_INCLUDE_DIRS})

# set variable pointing to the antlr tool that supports C++
# this is not required if the jar file can be found under PATH environment
set(ANTLR_EXECUTABLE ${PROJECT_SOURCE_DIR}/extern/antlr/antlr-4.9.1-complete.jar)
# add macros to generate ANTLR Cpp code from grammar
find_package(ANTLR REQUIRED)

# Call macro to add lexer and grammar to your build dependencies.
antlr_target(Foc Foc.g4 VISITOR PACKAGE foc)

# include generated files in project environment
include_directories(${ANTLR_Foc_OUTPUT_DIR})

# add generated grammar to foc binary target
file(GLOB sources src/*)
add_executable(foc main.cpp ${sources} ${ANTLR_Foc_CXX_OUTPUTS})
target_link_libraries(foc antlr4_static)

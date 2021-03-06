cmake_minimum_required (VERSION 2.8)
project (NISTfit)

# We heavily use C++11, C++11 support is required
set (CMAKE_CXX_STANDARD 11)

# All source files to be compiled shall have the .cpp file extension and reside in the src directory
file(GLOB_RECURSE APP_SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp")

# All global includes shall reside in the include directory
include_directories("${CMAKE_CURRENT_SOURCE_DIR}/include")

# Munge the arguments for g++ (might not be the best way, but seems to work)
if (${CMAKE_GENERATOR} MATCHES  "Unix Makefile")
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -pthread")
endif()

if (NISTFIT_EXAMPLES)
    add_executable(simple ${CMAKE_CURRENT_SOURCE_DIR}/examples/simple.cpp ${APP_SOURCES})
elseif (NISTFIT_STATIC_LIBRARY)
    add_library(NISTfit STATIC ${APP_SOURCES})
    if (CMAKE_VERSION VERSION_GREATER 3.0)
        # Add target include directories for easy linking with other applications
        target_include_directories(NISTfit PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include")
    endif()
endif()
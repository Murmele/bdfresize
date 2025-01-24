cmake_minimum_required(VERSION 3.10)

# Project name and version
project(bdfresize VERSION 1.5 LANGUAGES C)

# Set the C standard
set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED ON)

# Add compiler warnings (equivalent to Autotools)
if (CMAKE_C_COMPILER_ID STREQUAL "GNU" OR CMAKE_C_COMPILER_ID STREQUAL "Clang")
    add_compile_options(-Wall -Wunused -Wuninitialized -Wmissing-prototypes -Wmissing-declarations)
endif()

# Define the executable and its sources
add_executable(bdfresize
    bdfresize.c
    charresize.c
    getopt.c
)

target_compile_definitions(bdfresize PRIVATE VERSION="${PROJECT_VERSION}")

# Configure a header file to pass CMake settings to the source code
configure_file(${CMAKE_SOURCE_DIR}/config.h.in ${CMAKE_BINARY_DIR}/config.h)
target_include_directories(bdfresize PRIVATE ${CMAKE_BINARY_DIR})

# Add installation rules
install(TARGETS bdfresize DESTINATION bin)

# Extra distribution files (EXTRA_DIST in Autotools)
set_property(DIRECTORY APPEND PROPERTY ADDITIONAL_MAKE_CLEAN_FILES
    "def.h"
    "memset.c"
)

# Check for libraries and platform-specific dependencies (to be expanded as needed)
# Example:
# find_library(SOME_LIB NAMES somelib)
# target_link_libraries(bdfresize PRIVATE ${SOME_LIB})

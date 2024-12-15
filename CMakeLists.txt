cmake_minimum_required(VERSION 3.10)
project(NChant VERSION 0.1)

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Set build to debug
set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -g")

# Find Doxygen package
find_package(Doxygen)

if(DOXYGEN_FOUND)
    # Set Doxygen input and output directories
    set(DOXYGEN_INPUT_DIR ${PROJECT_SOURCE_DIR})
    set(DOXYGEN_OUTPUT_DIR ${PROJECT_BINARY_DIR}/docs)

    # Copy Doxyfile.in to build directory
    configure_file(
        ${PROJECT_SOURCE_DIR}/docs/Doxyfile.in
        ${PROJECT_BINARY_DIR}/Doxyfile @ONLY
    )

    # Add custom target for documentation
    add_custom_target(docs
        COMMAND ${DOXYGEN_EXECUTABLE} ${PROJECT_BINARY_DIR}/Doxyfile
        WORKING_DIRECTORY ${PROJECT_BINARY_DIR}
        COMMENT "Generating documentation with Doxygen"
        VERBATIM
    )
else()
    message(STATUS "Doxygen not found, documentation will not be built")
endif()

# Use NCurses
SET(CURSES_NEED_NCURSES TRUE)
# Use Unicode
SET(CURSES_NEED_WIDE TRUE)
# Find ncurses using CMake config
find_package(Curses REQUIRED)

# Add source files
set(SOURCES
    src/main.cpp
    src/core/Screen.cpp
    # src/core/Window.cpp
)

# Add include directories
include_directories(
    ${PROJECT_SOURCE_DIR}/include/core
    # ${PROJECT_SOURCE_DIR}/include/utils
    ${CURSES_INCLUDE_DIRS}
)

# Create main executable
add_executable(${PROJECT_NAME} ${SOURCES})

# Link against ncurses
target_link_libraries(${PROJECT_NAME} PRIVATE ${CURSES_LIBRARIES})

# Testing setup
# enable_testing()
# add_subdirectory(test)

cmake_minimum_required(VERSION 3.30)
project(goblin_RL CXX)

set(CMAKE_CXX_STANDARD 23)

# Include the command that downloads libraries
include(FetchContent)

# Define a function for adding git dependencies
function(include_dependency libName gitURL gitTag)
    # Setup the declaration
    FetchContent_Declare(${libName}
            GIT_REPOSITORY ${gitURL}
            GIT_TAG        ${gitTag}
            GIT_SHALLOW    TRUE
            GIT_PROGRESS   TRUE
    )
    FetchContent_MakeAvailable(${libName})
endfunction()

# Add RayLib support
set(LIB1 raylib)
find_package(${LIB1} QUIET)
if (NOT ${LIB1}_FOUND)
    message(STATUS "Getting ${LIB1} from Github")
    include_dependency(${LIB1} https://github.com/raysan5/raylib.git 5.5)
else()
    message(STATUS "Using local ${LIB1}")
endif()

add_executable(goblin_RL main.cpp)

# Set the include directory
target_include_directories(goblin_RL PRIVATE ${raylib_INCLUDE_DIRS})

# Link all libraries to the project
target_link_libraries(goblin_RL PRIVATE ${LIB1})

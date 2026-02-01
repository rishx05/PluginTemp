
cmake_minimum_required(VERSION 4.2.3 FATAL_ERROR)

project(AudioPlugin)

# Set the C++ standard only if this project is standalone;
# otherwise, let clients decide.
if(PROJECT_IS_TOP_LEVEL)
  if(NOT CMAKE_CXX_STANDARD)
    # Always use the newest C++ standard on green-field projects if possible.
    set(CMAKE_CXX_STANDARD 23)
  endif()

  # Links to C++ runtime statically
  set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
  # Embeds debug information
  set(CMAKE_MSVC_DEBUG_INFORMATION_FORMAT
      "$<$<CONFIG:Debug,RelWithDebInfo>:Embedded>"
      CACHE STRING "" FORCE
  )
endif()

# I like to download the dependencies to the same folder as the project.
# If you want to install them system wide, set CPM_SOURCE_CACHE with the path to the dependencies
# either as an environment variable or pass it to the cmake script with -DCPM_SOURCE_CACHE=<path>.
set(LIB_DIR ${CMAKE_CURRENT_SOURCE_DIR}/libs)

# Downloads CPM if not already downloaded. CPM is an easy-to-use package manager nicely integrated with CMake.
include(cmake/cpm.cmake)

cpmaddpackage(
  NAME JUCE
  GITHUB_REPOSITORY juce-framework/JUCE
  GIT_TAG 8.0.12
  VERSION 8.0.12
  SOURCE_DIR
  ${LIB_DIR}/juce
)


# Treat all Warnings as Errors
if (MSVC)
    add_compile_options(/Wall /WX)
else()
    add_compile_options(-Wall -Wextra -Wpedantic)
endif()

add_subdirectory(Plugin)


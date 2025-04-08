cmake_minimum_required(VERSION 3.14)
## https://iamsorush.com/posts/cpp-cmake-build/

project(test_yaml LANGUAGES C CXX VERSION 0.1 DESCRIPTION "Testing YAML and Workflow integration in GitHub")

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_VERBOSE_MAKEFILE ON)

if (LINUX)
    set(CMAKE_PREFIX_PATH "/usr/bin")
elseif(APPLE)
    set(CMAKE_OSX_ARCHITECTURES arm64;x86_64) # https://stackoverflow.com/questions/65157483/macos-build-universal-binary-2-with-cmake
    set(CMAKE_MACOSX_RPATH 1)
endif()


string (TOLOWER ${CMAKE_BUILD_TYPE} BUILD_TYPE_LOWERCASE) ## store the build type always as lower case and use it through out the cmake file


include_directories("${CMAKE_SOURCE_DIR}/src")

set(BASE_SRC
        src/test_yaml01.cpp
)

# used to declare the executable target. You can choose any name.
add_executable(${PROJECT_NAME})

### source files
## we add source files necessary to compile app target.
## PRIVATE: to say that sources are just for creating app target and nothing else.
target_sources(${PROJECT_NAME} PRIVATE
        ${BASE_SRC}
)


#################################
### Compiler Directives
#################################

if (APPLE)
    # We will aim to MacOS version 11.0 and up
    target_compile_options(${PROJECT_NAME} PUBLIC -mmacosx-version-min=11.0)
    target_link_libraries(${PROJECT_NAME} -mmacosx-version-min=11.0)
endif ()

install(TARGETS ${PROJECT_NAME})

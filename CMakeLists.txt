cmake_minimum_required(VERSION 3.5)
project(siyi-sdk-cpp)

SET(VERSION_MAJOR 0)
SET(VERSION_MINOR 1)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake")

set(CMAKE_CXX_STANDARD 20)

set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -Wpedantic -Wreturn-type -g -O2")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -Wall -Wpedantic -Wreturn-type -g -O0")

SET(LIBRARY_OUTPUT_PATH ${CMAKE_SOURCE_DIR}/lib)
SET(EXECUTABLE_OUTPUT_PATH ${CMAKE_SOURCE_DIR}/bin)

add_definitions(-g -D_FILE_OFFSET_BITS=64 -D_REENTRANT -DEXTERNAL_LOCKS -DMULTITHREAD)
add_definitions(-fPIC)
add_definitions(-Wno-deprecated)

include_directories(${CMAKE_SOURCE_DIR}/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include) # this is needed if used as submodule

file(GLOB LIB_SRCS src/*.cpp)
file(GLOB INCLUDE_FILES /include/siyi-sdk-cpp/*.h)

SET(SIYI_LIBS siyi_sdk_cpp_s glog pthread m)

ADD_LIBRARY(siyi_sdk_cpp_s STATIC ${LIB_SRCS} ${INCLUDE_FILES})

INSTALL(TARGETS siyi_sdk_cpp_s
            LIBRARY DESTINATION lib
            ARCHIVE DESTINATION lib
            )

INSTALL(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION include)

add_subdirectory(examples)

SET(EXECUTABLE_OUTPUT_PATH bin)
include(CTest)
enable_testing()
add_subdirectory(tests)


include(InstallRequiredSystemLibraries)
SET(CPACK_SET_DESTDIR True)
SET(CPACK_DEBIAN_PACKAGE_MAINTAINER "noname@dot.com")
set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/LICENSE")
set(CPACK_PACKAGE_VERSION_MAJOR "${VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${VERSION_MINOR}")
set(CPACK_SOURCE_GENERATOR "DEB")
include(CPack)




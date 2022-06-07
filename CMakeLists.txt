cmake_minimum_required (VERSION 3.9.0 FATAL_ERROR)

project(mcm LANGUAGES CXX C)

set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package(Threads REQUIRED)

# C++ flags and standarts
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set_property(GLOBAL PROPERTY USE_FOLDERS ON)

if ("${CMAKE_SYSTEM_NAME}" MATCHES "Linux" AND NOT EMSCRIPTEN AND NOT ANDROID)
	set(LINUX TRUE)
endif()

if (WIN32 AND IS_GCC_LIKE)
	add_definitions(-DWIN32)
endif()

if(WIN32)
  add_definitions(-DUNICODE -D_UNICODE)
  add_definitions(-D_WIN32_WINNT=0x0A00)
endif()

if(CMAKE_CXX_COMPILER_ID MATCHES "MSVC")
  if (CMAKE_BUILD_TYPE STREQUAL "Debug")
    set(debFlags "/Od")
  else()
    set(relFlags "/Ox /GT /GF /GS- /fp:fast /fp:except- /MP")
  endif()
else()
  set(relFlags "-msse2")
endif()

if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Choose Release or Debug" FORCE)
endif()

if (CMAKE_BUILD_TYPE STREQUAL "Release")
  set(CMAKE_C_FLAGS_RELEASE     "${CMAKE_C_FLAGS_RELEASE}  ${relFlags}")
  set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE}  ${relFlags}")
elseif(CMAKE_BUILD_TYPE STREQUAL "Debug")
  set(CMAKE_C_FLAGS_DEBUG     "${CMAKE_C_FLAGS_DEBUG}  ${debFlags}")
  set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG}  ${debFlags}")
endif() 


add_definitions(-DNDEBUG)
add_definitions(-D_FILE_OFFSET_BITS=64)

message("CMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}")
if (CMAKE_BUILD_TYPE STREQUAL "Release")
  message(${CMAKE_C_FLAGS_RELEASE})
  message(${CMAKE_CXX_FLAGS_RELEASE})
elseif(CMAKE_BUILD_TYPE STREQUAL "Debug")
  message(${CMAKE_C_FLAGS_DEBUG})
  message(${CMAKE_CXX_FLAGS_DEBUG})
endif()

add_subdirectory(src)



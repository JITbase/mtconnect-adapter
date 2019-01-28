cmake_minimum_required(VERSION 3.0.0 FATAL_ERROR)

################### Variables. ####################
# Change if you want modify path or other values. #
###################################################

set(PROJECT_NAME fanuc)
# Output Variables
set(OUTPUT_DEBUG Debug/bin)
set(OUTPUT_RELEASE Release/bin)
# Folders files
set(SOURCES_FILES FanucAdapter.cpp)
set(SRC_CPP ../src)
set(SRC_HEADER ../src)
set(PROJECT_CPP ./)
set(PROJECT_HEADER ./)
set(MININI_CPP ./minIni_07)
set(MININI_HEADER ../minIni_07)
set(FWLIB_CPP ../Fwlib)
set(FWLIB_HEADER ../Fwlib)



############## CMake Project ################
#        The main options of project        #
#############################################

project(${PROJECT_NAME} CXX)

# Define Release by default.
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE "Release")
  message(STATUS "Build type not specified: Use Release by default.")
endif(NOT CMAKE_BUILD_TYPE)

# Definition of Macros
add_definitions(
   -DINI_ANSIONLY
   -DDEBUG
   -DWIN32
   -DUNICODE
   -D_UNICODE
)

############## Artefacts Output #################
# Defines outputs , depending Debug or Release. #
#################################################

if(CMAKE_BUILD_TYPE STREQUAL "Debug")
  set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${OUTPUT_DEBUG}")
  set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${OUTPUT_DEBUG}")
  set(CMAKE_EXECUTABLE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${OUTPUT_DEBUG}")
else()
  set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${OUTPUT_REL}")
  set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${OUTPUT_REL}")
  set(CMAKE_EXECUTABLE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${OUTPUT_REL}")
endif()

################# Flags ################
# Defines Flags for Windows and Linux. #
########################################

if(MSVC)
   set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /W3 /EHsc")
   set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /W3 /EHsc")
endif(MSVC)
if(NOT MSVC)
   set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
   if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
       set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -stdlib=libc++")
   endif()
endif(NOT MSVC)

################ Files ################
#   --   Add files to project.   --   #
#######################################

include_directories(SYSTEM ${SRC_HEADER})
include_directories(SYSTEM ${PROJECT_HEADER})
include_directories(SYSTEM ${MININI_HEADER})
include_directories(SYSTEM ${FWLIB_HEADER})

file(GLOB SOURCES_FILES
    ${SRC_CPP}/*.cpp
    ${SRC_CPP}/*.c
    ${SRC_HEADER}/*.h
    ${SRC_HEADER}/*.hpp
    ${PROJECT_CPP}/*.cpp
    ${PROJECT_CPP}/*.c
    ${PROJECT_HEADER}/*.h
    ${PROJECT_HEADER}/*.hpp
    ${MININI_CPP}/*.cpp
    ${MININI_CPP}/*.c
    ${MININI_HEADER}/*.h
    ${MININI_HEADER}/*.hpp
    ${FWLIB_CPP}/*.cpp
    ${FWLIB_CPP}/*.c
    ${FWLIB_HEADER}/*.h
    ${FWLIB_HEADER}/*.hpp
)

# Ignore unused files
list(REMOVE_ITEM SOURCES_FILES
   ${SRC_CPP}/configuration.cpp
   ${SRC_HEADER}/configuration.hpp
)

# Add executable to build.
add_executable(${PROJECT_NAME}
   ${SOURCES_FILES}
)

if(MSVC)
   target_link_libraries(${PROJECT_NAME} ..\Fwlib\Fwlib32.lib wsock32.lib )
endif(MSVC)

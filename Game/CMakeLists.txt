{{=<% %>=}}

cmake_minimum_required(VERSION 2.8)

project(<% gameName %>)

include (${RAFT})

#Set C++ Version to 11
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -g -Wall")

if("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU" AND NOT ANDROID)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fsanitize=address")
endif()

set (GAME_DEPS

    Ancona_Platformer
    Ancona_Core2D
    Ancona_Framework
    Ancona_Util2D
    Ancona_Util
    Ancona_System

    sfml-graphics
    sfml-window
    sfml-system

    jsoncpp)

add_subdirectory(Src/<% gameName %>/<% gameAbbr %>Lib)

if(RAFT_IS_ANDROID)
    set(AssetOutputDir ${CMAKE_BINARY_DIR}/Android/<% gameName %>/assets)
    set(GAME_SRC Src/<% gameName %>/Platforms/Android/main.cpp)
    set(GAME_DEPS ${GAME_DEPS} -Wl,--whole-archive sfml-main -Wl,--no-whole-archive EGL android log)
    file(COPY resources/ DESTINATION ${AssetOutputDir}/resources/)

    add_library(<% gameName %> SHARED ${GAME_SRC})
elseif(RAFT_IS_DESKTOP)
    set(AssetOutputDir ${CMAKE_BINARY_DIR}/)
    set(RESOURCE_DIR_LOCATION ${CMAKE_CURRENT_SOURCE_DIR}/)
    set(GAME_SRC Src/<% gameName %>/Platforms/Desktop/main.cpp)

    add_executable(<% gameName %> ${GAME_SRC})
endif()

target_link_libraries(<% gameName %> <%gameAbbr%>_Lib)
target_link_libraries(<% gameName %> ${GAME_DEPS})

configure_file(Config.txt ${AssetOutputDir}/Config.txt)
configure_file(save.dat ${AssetOutputDir}/save.dat COPYONLY)
configure_file(project/collision-types.json ${AssetOutputDir}/project/collision-types.json COPYONLY)
file(COPY maps/ DESTINATION ${AssetOutputDir}/maps/)

cmake_minimum_required(VERSION 3.0.0)

project(liboact)

if(WIN32)
    add_definitions(-DNOMINMAX)
    add_definitions(-D_CRT_SECURE_NO_WARNINGS)
endif()

list(APPEND oact_src_files
    ./include/oact/ActionManager.h
    ./src/ActionManager.cpp
)

add_library(${PROJECT_NAME} STATIC ${oact_src_files})
target_include_directories(${PROJECT_NAME} PUBLIC ./include PRIVATE ./src)

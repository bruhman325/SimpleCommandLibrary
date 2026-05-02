
cmake_minimum_required(VERSION 3.12)

add_library(SCL)

target_sources(SCL
    PRIVATE
    SCL.cpp
    PUBLIC 
        FILE_SET dataheaders
        TYPE HEADERS
        FILES
	SCL.h 
)


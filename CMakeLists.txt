cmake_minimum_required (VERSION 2.8)

set(libname "sole")
project(${libname} CXX)

if (MSVC)
	add_definitions(-D_CRT_SECURE_NO_WARNINGS)
endif()

set(hdr_public
	"include/sole/sole.hpp")

add_library(${libname} STATIC 
	sole.cpp
	${hdr_public})

set_target_properties(${libname} PROPERTIES
	PUBLIC_HEADER "${hdr_public}"
	INCLUDE_DIRECTORIES "${CMAKE_CURRENT_SOURCE_DIR}/include")

include_directories(${CMAKE_CURRENT_SOURCE_DIR})

install(TARGETS ${libname}
	LIBRARY DESTINATION lib COMPONENT lib
	ARCHIVE DESTINATION lib COMPONENT lib
	RUNTIME DESTINATION bin COMPONENT bin
	PUBLIC_HEADER DESTINATION include COMPONENT dev)
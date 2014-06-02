cmake_minimum_required (VERSION 2.8)

set(libname "sole")
project(${libname} CXX)

option(BUILD_TESTS "Build tests" OFF)

if (MSVC)
	add_definitions(-D_CRT_SECURE_NO_WARNINGS)
endif()

set(hdr_public
	"include/sole/sole.hpp")

set(test_src
	"tests.cc")

add_library(${libname} STATIC 
	sole.cpp
	${hdr_public})

set_target_properties(${libname} PROPERTIES
	PUBLIC_HEADER "${hdr_public}"
	INCLUDE_DIRECTORIES "${CMAKE_CURRENT_SOURCE_DIR}/include")

if(BUILD_TESTS)
	add_executable(tests ${test_src})
	set_target_properties(tests PROPERTIES
		INCLUDE_DIRECTORIES "${CMAKE_CURRENT_SOURCE_DIR}/include")
	target_link_libraries(tests ${libname})
endif()

install(TARGETS ${libname}
	LIBRARY DESTINATION lib COMPONENT lib
	ARCHIVE DESTINATION lib COMPONENT lib
	RUNTIME DESTINATION bin COMPONENT bin
	PUBLIC_HEADER DESTINATION include COMPONENT dev)
cmake_minimum_required (VERSION 2.8)

set(libname "sole")
project(${libname} CXX)

option(BUILD_SOLE_TESTS "Build sole tests" ON)

if (MSVC)
	add_definitions(-D_CRT_SECURE_NO_WARNINGS)
endif()

if(CMAKE_COMPILER_IS_GNUCXX)
	set(CMAKE_CXX_FLAGS "-std=c++11")
endif()

set(hdr_public
	"include/sole/sole.hpp")

set(test_src
	"tests.cc")

list(APPEND include_dirs "${CMAKE_CURRENT_SOURCE_DIR}/include")

add_library(${libname} STATIC 
	sole.cpp
	${hdr_public})

set_target_properties(${libname} PROPERTIES
	PUBLIC_HEADER "${hdr_public}"
	INTERFACE_INCLUDE_DIRECTORIES "${include_dirs}"
	INCLUDE_DIRECTORIES "${include_dirs}")

if(BUILD_SOLE_TESTS)
	add_executable(sole_tests EXCLUDE_FROM_ALL ${test_src})
	target_link_libraries(sole_tests ${libname})
endif()

install(TARGETS ${libname}
	LIBRARY DESTINATION lib COMPONENT lib
	ARCHIVE DESTINATION lib COMPONENT lib
	RUNTIME DESTINATION bin COMPONENT bin
	PUBLIC_HEADER DESTINATION include/sole COMPONENT dev)

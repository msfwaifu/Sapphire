cmake_minimum_required(VERSION 2.6)
cmake_policy(SET CMP0015 NEW)
project(Sapphire)

file(GLOB SERVER_PUBLIC_INCLUDE_FILES "${CMAKE_CURRENT_SOURCE_DIR}/*")
file(GLOB SERVER_SOURCE_FILES "${CMAKE_CURRENT_SOURCE_DIR}*.c*")

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY  "${CMAKE_CURRENT_SOURCE_DIR}/../../../bin/")
add_executable(server_rest ${SERVER_PUBLIC_INCLUDE_FILES} ${SERVER_SOURCE_FILES})
add_dependencies(server_rest Common xivdat)

set_target_properties(server_rest PROPERTIES
    CXX_STANDARD 14
    CXX_STANDARD_REQUIRED ON
    CXX_EXTENSIONS ON
    RUNTIME_OUTPUT_DIRECTORY_DEBUG "${CMAKE_CURRENT_SOURCE_DIR}/../../../bin/"
    RUNTIME_OUTPUT_DIRECTORY_RELEASE "${CMAKE_CURRENT_SOURCE_DIR}/../../../bin/"
    RUNTIME_OUTPUT_DIRECTORY_RELWITHDEBINFO "${CMAKE_CURRENT_SOURCE_DIR}/../../../bin/"
    RUNTIME_OUTPUT_DIRECTORY_MINSIZEREL "${CMAKE_CURRENT_SOURCE_DIR}/../../../bin/"
)

if(MSVC)
    message(STATUS "Enabling Edit and Continue..")
    set_property(TARGET server_rest APPEND_STRING PROPERTY COMPILE_FLAGS " /INCREMENTAL /ZI")
endif()

if (UNIX)
  target_link_libraries (server_rest Common xivdat pthread mysqlclient dl z)
else()
  target_link_libraries (server_rest Common xivdat libmysql zlib1)
endif()

target_link_libraries( server_rest ${Boost_LIBRARIES} ${Boost_LIBRARIES} )

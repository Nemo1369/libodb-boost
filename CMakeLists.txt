cmake_minimum_required(VERSION 2.8.12)

cmake_policy(SET CMP0042 NEW)
cmake_policy(SET CMP0048 NEW)

project(odb-boost CXX C)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake" "${CMAKE_SOURCE_DIR}/cmake/modules")
set(CMAKE_EXPORT_COMPILE_COMMANDS TRUE)

include(TargetArchitecture)

target_architecture(CMAKE_TARGET_ARCHITECTURE)

if(MSVC AND NOT EXISTS "${CMAKE_BINARY_DIR}/ALL_BUILD.vcxproj.user")
    file(GENERATE
            OUTPUT "${CMAKE_BINARY_DIR}/ALL_BUILD.vcxproj.user"
            INPUT "${CMAKE_SOURCE_DIR}/cmake/ALL_BUILD.vcxproj.user.in"
            )
endif()

option(BUILD_UNIT_TESTS "Build unit tests" FALSE)
option(BUILD_SHARED_LIBRARIES "Build shared library version" TRUE)
option(BUILD_WITH_DATE_TIME "Build with boost::date_time support" FALSE)
option(BUILD_WITH_MULTI_INDEX "Build with boost::multi_index support" FALSE)
option(BUILD_WITH_OPTIONAL "Build with boost::optional support" FALSE)
option(BUILD_WITH_SMART_PTR "Build with boost::smart_ptr support" FALSE)
option(BUILD_WITH_UUID "Build with boost::uuid support" FALSE)

set(ODB_LIBODB_SOURCES_DIR "" CACHE STRING "libodb source directory path")
set(ODB_MSSQL_SOURCES_DIR "" CACHE STRING "libodb-mssql source directory path")
set(ODB_MYSQL_SOURCES_DIR "" CACHE STRING "libodb-mysql source directory path")
set(ODB_ORACLE_SOURCES_DIR "" CACHE STRING "libodb-oracle source directory path")
set(ODB_PGSQL_SOURCES_DIR "" CACHE STRING "libodb-pgsql source directory path")
set(ODB_SQLITE_SOURCES_DIR "" CACHE STRING "libodb-sqlite source directory path")

set(APPLE_CODE_SIGN_IDENTITY "" CACHE STRING "Sign identity name")

set(CMAKE_OSX_ARCHITECTURES "$(ARCHS_STANDARD)" CACHE string "Build architecture")

if(${CMAKE_C_COMPILER_ID} MATCHES "Clang" OR ${CMAKE_CXX_COMPILER_ID} MATCHES "Clang")
    set(CMAKE_COMPILER_IS_CLANG TRUE)
endif()

if(CMAKE_COMPILER_IS_GNUCC OR CMAKE_COMPILER_IS_GNUCXX OR CMAKE_COMPILER_IS_CLANG)
    option(USE_LIBC++ "Use libc++ instead of libstdc++" ${APPLE})
endif()

find_package(ODB)

if(NOT ${CMAKE_TARGET_ARCHITECTURE} STREQUAL ${CMAKE_HOST_SYSTEM_PROCESSOR} OR NOT ODB_LIBODB_FOUND)
    if(NOT TARGET odb)
        get_filename_component(ABSOLUTE_LIBODB_SOURCES_DIR ${ODB_LIBODB_SOURCES_DIR} ABSOLUTE)
        get_filename_component(LIBODB_SOURCES_DIR_NAME ${ODB_LIBODB_SOURCES_DIR}
                NAME)

        add_subdirectory(${ABSOLUTE_LIBODB_SOURCES_DIR}
                ${CMAKE_BINARY_DIR}/${LIBODB_SOURCES_DIR_NAME})
    endif()
endif()

configure_file(${PROJECT_SOURCE_DIR}/odb/boost/details/config.h.in
        ${PROJECT_SOURCE_DIR}/odb/boost/details/config.h)

if(NOT TARGET boost::date_time AND ${CMAKE_TARGET_ARCHITECTURE} STREQUAL ${CMAKE_HOST_SYSTEM_PROCESSOR})
    find_package(Boost COMPONENTS date_time)
endif()

find_package(ODB OPTIONAL_COMPONENTS mssql mysql pgsql oracle sqlite)

if(NOT ${CMAKE_TARGET_ARCHITECTURE} STREQUAL ${CMAKE_HOST_SYSTEM_PROCESSOR} OR NOT ODB_LIBODB_FOUND)
    if(NOT TARGET odb AND ODB_LIBODB_SOURCES_DIR)
        get_filename_component(ABSOLUTE_LIBODB_SOURCES_DIR ${ODB_LIBODB_SOURCES_DIR} ABSOLUTE)
        get_filename_component(LIBODB_SOURCES_DIR_NAME ${ODB_LIBODB_SOURCES_DIR}
                NAME)

        add_subdirectory(${ABSOLUTE_LIBODB_SOURCES_DIR}
                ${CMAKE_BINARY_DIR}/${LIBODB_SOURCES_DIR_NAME})
    endif()
endif()

if(NOT ${CMAKE_TARGET_ARCHITECTURE} STREQUAL ${CMAKE_HOST_SYSTEM_PROCESSOR} OR NOT ODB_MSSQL_FOUND)
    if(NOT TARGET odb-mssql AND ODB_MSSQL_SOURCES_DIR)
        get_filename_component(ABSOLUTE_MSSQL_SOURCES_DIR ${ODB_MSSQL_SOURCES_DIR} ABSOLUTE)
        get_filename_component(MSSQL_SOURCES_DIR_NAME ${ODB_MSSQL_SOURCES_DIR}
                NAME)

        add_subdirectory(${ABSOLUTE_MSSQL_SOURCES_DIR}
                ${CMAKE_BINARY_DIR}/${MSSQL_SOURCES_DIR_NAME})
    endif()
endif()

if(NOT ${CMAKE_TARGET_ARCHITECTURE} STREQUAL ${CMAKE_HOST_SYSTEM_PROCESSOR} OR NOT ODB_MYSQL_FOUND)
    if(NOT TARGET odb-mysql AND ODB_MYSQL_SOURCES_DIR)
        get_filename_component(ABSOLUTE_MYSQL_SOURCES_DIR ${ODB_MYSQL_SOURCES_DIR} ABSOLUTE)
        get_filename_component(MYSQL_SOURCES_DIR_NAME ${ODB_MYSQL_SOURCES_DIR}
                NAME)

        add_subdirectory(${ABSOLUTE_MYSQL_SOURCES_DIR}
                ${CMAKE_BINARY_DIR}/${MYSQL_SOURCES_DIR_NAME})
    endif()
endif()

if(NOT ${CMAKE_TARGET_ARCHITECTURE} STREQUAL ${CMAKE_HOST_SYSTEM_PROCESSOR} OR NOT ODB_PGSQL_FOUND)
    if(NOT TARGET odb-pgsql AND ODB_PGSQL_SOURCES_DIR)
        get_filename_component(ABSOLUTE_PGSQL_SOURCES_DIR ${ODB_PGSQL_SOURCES_DIR} ABSOLUTE)
        get_filename_component(PGSQL_SOURCES_DIR_NAME ${ODB_PGSQL_SOURCES_DIR}
                NAME)

        add_subdirectory(${ABSOLUTE_PGSQL_SOURCES_DIR}
                ${CMAKE_BINARY_DIR}/${PGSQL_SOURCES_DIR_NAME})
    endif()
endif()

if(NOT ${CMAKE_TARGET_ARCHITECTURE} STREQUAL ${CMAKE_HOST_SYSTEM_PROCESSOR} OR NOT ODB_ORACLE_FOUND)
    if(NOT TARGET odb-oracle AND ODB_ORACLE_SOURCES_DIR)
        get_filename_component(ABSOLUTE_ORACLE_SOURCES_DIR ${ODB_ORACLE_SOURCES_DIR} ABSOLUTE)
        get_filename_component(ORACLE_SOURCES_DIR_NAME ${ODB_ORACLE_SOURCES_DIR}
                NAME)

        add_subdirectory(${ABSOLUTE_ORACLE_SOURCES_DIR}
                ${CMAKE_BINARY_DIR}/${ORACLE_SOURCES_DIR_NAME})
    endif()
endif()

if(NOT ${CMAKE_TARGET_ARCHITECTURE} STREQUAL ${CMAKE_HOST_SYSTEM_PROCESSOR} OR NOT ODB_SQLITE_FOUND)
    if(NOT TARGET odb-sqlite AND ODB_SQLITE_SOURCES_DIR)
        get_filename_component(ABSOLUTE_SQLITE_SOURCES_DIR ${ODB_SQLITE_SOURCES_DIR} ABSOLUTE)
        get_filename_component(SQLITE_SOURCES_DIR_NAME ${ODB_SQLITE_SOURCES_DIR}
                NAME)

        add_subdirectory(${ABSOLUTE_SQLITE_SOURCES_DIR}
                ${CMAKE_BINARY_DIR}/${SQLITE_SOURCES_DIR_NAME})
    endif()
endif()

if(BUILD_UNIT_TESTS)
    enable_testing()
endif()

add_subdirectory(odb)
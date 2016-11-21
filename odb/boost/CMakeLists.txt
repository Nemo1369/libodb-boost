set(CURRENT_TARGET odb-boost)

list(APPEND ${CURRENT_TARGET}_INCLUDES
        ${PROJECT_SOURCE_DIR}
        )

if(ODB_LIBODB_FOUND)
    list(APPEND ${CURRENT_TARGET}_INCLUDES
            ${ODB_LIBODB_INCLUDE_DIRS}
            )

    list(APPEND ${CURRENT_TARGET}_LIBRARIES
            ${ODB_LIBODB_LIBRARIES}
            )
else()
    list(APPEND ${CURRENT_TARGET}_INCLUDES
            ${ODB_LIBODB_SOURCES_DIR}
            )

    list(APPEND ${CURRENT_TARGET}_LIBRARIES odb)
endif()

list(APPEND ${CURRENT_TARGET}_UNGROUPED_HEADERS
        exception.hxx
        lazy-ptr.hxx
        version.hxx
        )

list(APPEND ${CURRENT_TARGET}_DATE_TIME_HEADERS
        date-time/exceptions.hxx
        )

list(APPEND ${CURRENT_TARGET}_DATE_TIME_SOURCES
        date-time/exceptions.cxx
        )

list(APPEND ${CURRENT_TARGET}_DATE_TIME_MSSQL_HEADERS
        date-time/mssql/gregorian-mapping.hxx
        date-time/mssql/gregorian-traits.hxx
        date-time/mssql/posix-time-mapping.hxx
        date-time/mssql/posix-time-traits.hxx
        )

list(APPEND ${CURRENT_TARGET}_DATE_TIME_MYSQL_HEADERS
        date-time/mysql/gregorian-mapping.hxx
        date-time/mysql/gregorian-traits.hxx
        date-time/mysql/posix-time-mapping.hxx
        date-time/mysql/posix-time-traits.hxx
        )

list(APPEND ${CURRENT_TARGET}_DATE_TIME_ORACLE_HEADERS
        date-time/oracle/gregorian-mapping.hxx
        date-time/oracle/gregorian-traits.hxx
        date-time/oracle/posix-time-mapping.hxx
        date-time/oracle/posix-time-traits.hxx
        )

list(APPEND ${CURRENT_TARGET}_DATE_TIME_PGSQL_HEADERS
        date-time/pgsql/gregorian-mapping.hxx
        date-time/pgsql/gregorian-traits.hxx
        date-time/pgsql/posix-time-mapping.hxx
        date-time/pgsql/posix-time-traits.hxx
        )

list(APPEND ${CURRENT_TARGET}_DATE_TIME_SQLITE_HEADERS
        date-time/sqlite/gregorian-mapping.hxx
        date-time/sqlite/gregorian-traits.hxx
        date-time/sqlite/posix-time-mapping.hxx
        date-time/sqlite/posix-time-traits.hxx
        )

list(APPEND ${CURRENT_TARGET}_MULTI_INDEX_HEADERS
        multi-index/container-traits.hxx
        )

list(APPEND ${CURRENT_TARGET}_OPTIONAL_HEADERS
        optional/wrapper-traits.hxx
        )

list(APPEND ${CURRENT_TARGET}_SMART_PTR_HEADERS
        smart-ptr/lazy-pointer-traits.hxx
        smart-ptr/lazy-ptr.hxx
        smart-ptr/lazy-ptr.ixx
        smart-ptr/lazy-ptr.txx
        smart-ptr/pointer-traits.hxx
        smart-ptr/wrapper-traits.hxx
        )

list(APPEND ${CURRENT_TARGET}_UNORDERED_HEADERS
        unordered/container-traits.hxx
        )

list(APPEND ${CURRENT_TARGET}_UUID_MSSQL_HEADERS
        uuid/mssql/uuid-mapping.hxx
        uuid/mssql/uuid-traits.hxx
        )

list(APPEND ${CURRENT_TARGET}_UUID_MYSQL_HEADERS
        uuid/mysql/uuid-mapping.hxx
        uuid/mysql/uuid-traits.hxx
        )

list(APPEND ${CURRENT_TARGET}_UUID_ORACLE_HEADERS
        uuid/oracle/uuid-mapping.hxx
        uuid/oracle/uuid-traits.hxx
        )

list(APPEND ${CURRENT_TARGET}_UUID_PGSQL_HEADERS
        uuid/pgsql/uuid-mapping.hxx
        uuid/pgsql/uuid-traits.hxx
        )

list(APPEND ${CURRENT_TARGET}_UUID_SQLITE_HEADERS
        uuid/sqlite/uuid-mapping.hxx
        uuid/sqlite/uuid-traits.hxx
        )

list(APPEND ${CURRENT_TARGET}_DETAILS_HEADERS
        details/config.hxx
        details/export.hxx
        )

list(APPEND ${CURRENT_TARGET}_UNGROUPED_SOURCES

        )

list(APPEND ${CURRENT_TARGET}_HEADERS
        ${${CURRENT_TARGET}_UNGROUPED_HEADERS}
        ${${CURRENT_TARGET}_DETAILS_HEADERS}
        )

if(Boost_FOUND OR TARGET boost::date_time)
    if(BUILD_WITH_DATE_TIME)
        list(APPEND ${CURRENT_TARGET}_HEADERS
                ${${CURRENT_TARGET}_DATE_TIME_HEADERS}
                )

        list(APPEND ${CURRENT_TARGET}_SOURCES
                ${${CURRENT_TARGET}_DATE_TIME_SOURCES}
                )

        if(ODB_MSSQL_FOUND OR TARGET odb-mssql)
            list(APPEND ${CURRENT_TARGET}_HEADERS
                    ${${CURRENT_TARGET}_DATE_TIME_MSSQL_HEADERS}
                    )
        endif()

        if(ODB_MYSQL_FOUND OR TARGET odb-mysql)
            list(APPEND ${CURRENT_TARGET}_HEADERS
                    ${${CURRENT_TARGET}_DATE_TIME_MYSQL_HEADERS}
                    )
        endif()

        if(ODB_ORACLE_FOUND OR TARGET odb-oracle)
            list(APPEND ${CURRENT_TARGET}_HEADERS
                    ${${CURRENT_TARGET}_DATE_TIME_ORACLE_HEADERS}
                    )
        endif()

        if(ODB_PGSQL_FOUND OR TARGET odb-pgsql)
            list(APPEND ${CURRENT_TARGET}_HEADERS
                    ${${CURRENT_TARGET}_DATE_TIME_PGSQL_HEADERS}
                    )
        endif()

        if(ODB_SQLITE_FOUND OR TARGET odb-sqlite)
            list(APPEND ${CURRENT_TARGET}_HEADERS
                    ${${CURRENT_TARGET}_DATE_TIME_SQLITE_HEADERS}
                    )
        endif()
    endif()
endif()

if(Boost_FOUND OR TARGET boost::multi_index)
    if(BUILD_WITH_MULTI_INDEX)
        message("!")
        list(APPEND ${CURRENT_TARGET}_HEADERS
                ${${CURRENT_TARGET}_MULTI_INDEX_HEADERS}
                )
    endif()
endif()

if(Boost_FOUND OR TARGET boost::optional)
    if(BUILD_WITH_OPTIONAL)
        list(APPEND ${CURRENT_TARGET}_HEADERS
                ${${CURRENT_TARGET}_OPTIONAL_HEADERS}
                )
    endif()
endif()

if(Boost_FOUND OR TARGET boost::smart_ptr)
    if(BUILD_WITH_SMART_PTR)
        list(APPEND ${CURRENT_TARGET}_HEADERS
                ${${CURRENT_TARGET}_SMART_PTR_HEADERS}
                )
    endif()
endif()

if(Boost_FOUND OR TARGET boost::unordered)
    if(BUILD_WITH_UNORDERED)
        list(APPEND ${CURRENT_TARGET}_HEADERS
                ${${CURRENT_TARGET}_UNORDERED_HEADERS}
                )
    endif()
endif()

if(Boost_FOUND OR TARGET boost::uuid)
    if(BUILD_WITH_UUID)
        if(ODB_MSSQL_FOUND OR TARGET odb-mssql)
            list(APPEND ${CURRENT_TARGET}_HEADERS
                    ${${CURRENT_TARGET}_UUID_MSSQL_HEADERS}
                    )
        endif()

        if(ODB_MYSQL_FOUND OR TARGET odb-mysql)
            list(APPEND ${CURRENT_TARGET}_HEADERS
                    ${${CURRENT_TARGET}_UUID_MYSQL_HEADERS}
                    )
        endif()

        if(ODB_ORACLE_FOUND OR TARGET odb-oracle)
            list(APPEND ${CURRENT_TARGET}_HEADERS
                    ${${CURRENT_TARGET}_UUID_ORACLE_HEADERS}
                    )
        endif()

        if(ODB_PGSQL_FOUND OR TARGET odb-pgsql)
            list(APPEND ${CURRENT_TARGET}_HEADERS
                    ${${CURRENT_TARGET}_UUID_PGSQL_HEADERS}
                    )
        endif()

        if(ODB_SQLITE_FOUND OR TARGET odb-sqlite)
            list(APPEND ${CURRENT_TARGET}_HEADERS
                    ${${CURRENT_TARGET}_UUID_SQLITE_HEADERS}
                    )
        endif()
    endif()
endif()

list(APPEND ${CURRENT_TARGET}_SOURCES
        ${${CURRENT_TARGET}_UNGROUPED_SOURCES}
        )

if(BUILD_SHARED_LIBRARIES)
    add_library(${CURRENT_TARGET} SHARED
            ${${CURRENT_TARGET}_HEADERS}
            ${${CURRENT_TARGET}_SOURCES}
            )
else()
    add_library(${CURRENT_TARGET} STATIC
            ${${CURRENT_TARGET}_HEADERS}
            ${${CURRENT_TARGET}_SOURCES}
            )
endif()

target_include_directories(${CURRENT_TARGET} PUBLIC
        "$<BUILD_INTERFACE:${${CURRENT_TARGET}_INCLUDES}>"
        )

target_link_libraries(${CURRENT_TARGET} ${${CURRENT_TARGET}_LIBRARIES})

if(CMAKE_COMPILER_IS_GNUCC OR CMAKE_COMPILER_IS_GNUCXX OR CMAKE_COMPILER_IS_CLANG)

    target_compile_options(${CURRENT_TARGET} PUBLIC $<$<COMPILE_LANGUAGE:C>:-Wall -Wextra -Wno-unused-function -Wno-unused-parameter -Werror-implicit-function-declaration -Wno-missing-braces -Wno-missing-field-initializers -std=gnu99 -fno-strict-aliasing>)

    if(USE_LIBC++)
        target_compile_options(${CURRENT_TARGET} PUBLIC $<$<COMPILE_LANGUAGE:CXX>:-Wall -Wextra -Wno-unused-function -Wno-unused-parameter -Werror-implicit-function-declaration -Wno-missing-field-initializers -fno-strict-aliasing -std=gnu++14 -stdlib=libc++>)
    else()
        target_compile_options(${CURRENT_TARGET} PUBLIC $<$<COMPILE_LANGUAGE:CXX>:-Wall -Wextra -Wno-unused-function -Wno-unused-parameter -Werror-implicit-function-declaration -Wno-missing-field-initializers -fno-strict-aliasing -std=gnu++14>)
    endif()

    target_compile_options(${CURRENT_TARGET} PUBLIC $<$<CONFIG:DEBUG>:-g>)
endif()

set_target_properties(${CURRENT_TARGET} PROPERTIES LINKER_LANGUAGE CXX)
set_target_properties(${CURRENT_TARGET} PROPERTIES ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})
set_target_properties(${CURRENT_TARGET} PROPERTIES BINARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})
set_target_properties(${CURRENT_TARGET} PROPERTIES LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})

if(APPLE AND NOT ${CMAKE_TARGET_ARCHITECTURE} EQUAL ${CMAKE_HOST_SYSTEM_PROCESSOR})
    set_target_properties(${CURRENT_TARGET} PROPERTIES XCODE_ATTRIBUTE_CODE_SIGN_IDENTITY ${APPLE_SIGN_IDENTITY})
endif()
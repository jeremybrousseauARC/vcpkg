cmake_minimum_required(VERSION 3.5)
project(rhash C)

file(STRINGS ${CMAKE_CURRENT_SOURCE_DIR}/Makefile RHASH_SOURCES REGEX "^SOURCES = .*$")
string(REPLACE "SOURCES = " "" RHASH_SOURCES "${RHASH_SOURCES}")
string(REPLACE " " ";" RHASH_SOURCES "${RHASH_SOURCES}")
add_definitions(-DRHASH_XVERSION="${RHASH_XVERSION}")
add_library(rhash ${RHASH_SOURCES})
set_target_properties(rhash PROPERTIES COMPILE_DEFINITIONS "IN_RHASH" DEFINE_SYMBOL "RHASH_EXPORTS")

target_include_directories(rhash INTERFACE $<INSTALL_INTERFACE:include>)

install(TARGETS rhash EXPORT unofficial-rhash-config
      RUNTIME DESTINATION bin
      LIBRARY DESTINATION lib
      ARCHIVE DESTINATION lib
)

install(EXPORT unofficial-rhash-config
        FILE unofficial-rhash-config.cmake
        NAMESPACE unofficial::rhash::
        DESTINATION share/unofficial-rhash
)

if(NOT RHASH_SKIP_HEADERS)
    install(FILES rhash.h rhash_torrent.h DESTINATION include)
endif()

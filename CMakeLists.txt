cmake_minimum_required(VERSION 3.0)

set(TARGET atinout)
set(TARGET_VERSION "0.9.1")

project(${TARGET})

set(CMAKE_C_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Werror")

add_definitions(-DVERSION=\"${TARGET_VERSION}\")
add_executable( ${TARGET} atinout.c)

install(TARGETS ${TARGET} DESTINATION bin)

if (NOT DEFINED PACKAGE_ARCHITECTURE)
    execute_process (
            COMMAND dpkg --print-architecture
            OUTPUT_VARIABLE PACKAGE_ARCHITECTURE
            OUTPUT_STRIP_TRAILING_WHITESPACE
    )
endif()

set(CPACK_GENERATOR "DEB")
set(CPACK_PACKAGE_NAME "${TARGET}")
set(CPACK_PACKAGE_VERSION "${TARGET_VERSION}")
set(CPACK_PACKAGE_FILE_NAME "${CPACK_PACKAGE_NAME}_${CPACK_PACKAGE_VERSION}_${PACKAGE_ARCHITECTURE}")
set(CPACK_DEBIAN_PACKAGE_SECTION "comm")
set(CPACK_DEBIAN_PACKAGE_MAINTAINER "Vadim Cherenev <sl@anhot.ru>")
set(CPACK_DEBIAN_PACKAGE_DESCRIPTION "AT commands as input are sent to modem and responses given as output")
set(CPACK_DEBIAN_PACKAGE_DEPENDS "")
set(CPACK_DEBIAN_PACKAGE_ARCHITECTURE "${PACKAGE_ARCHITECTURE}")
set(CPACK_PACKAGING_INSTALL_PREFIX "usr")
set(CPACK_DEBIAN_PACKAGE_HOMEPAGE "http://atinout.sourceforge.net/")
set(CPACK_DEBIAN_PACKAGE_CONTROL_EXTRA
        ${CMAKE_SOURCE_DIR}/deb/postinst
        ${CMAKE_SOURCE_DIR}/deb/prerm
        ${CMAKE_SOURCE_DIR}/deb/postrm)

include(CPack)
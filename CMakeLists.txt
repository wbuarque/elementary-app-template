project (com.wbuarque.elementary-app)

cmake_minimum_required (VERSION 2.6)

set (GETTEXT_PACKAGE "${CMAKE_PROJECT_NAME}")
add_definitions (-DGETTEXT_PACKAGE=\"${GETTEXT_PACKAGE}\")
add_subdirectory (po)

list (APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake)
list (APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/src)

set (DATADIR "${CMAKE_INSTALL_PREFIX}/share")

set (PKGDATADIR "${DATADIR}/com.wbuarque.elementary-app")

set (EXEC_NAME "com.wbuarque.elementary-app")
set (RELEASE_NAME "A template app.")
set (VERSION "0.1")
set (VERSION_INFO "Version Info")

find_package(PkgConfig)

pkg_check_modules(DEPS REQUIRED gtk+-3.0)

add_definitions(${DEPS_CFLAGS})
link_libraries(${DEPS_LIBRARIES})
link_directories(${DEPS_LIBRARY_DIRS})

find_package(Vala REQUIRED)
include(ValaVersion)
ensure_vala_version("0.36" MINIMUM)

include(ValaPrecompile)
vala_precompile(VALA_C ${EXEC_NAME}
    src/template.vala

PACKAGES
    gtk+-3.0
)

add_executable(${EXEC_NAME} ${VALA_C})

install (TARGETS ${EXEC_NAME} RUNTIME DESTINATION bin)
install (FILES ${CMAKE_CURRENT_SOURCE_DIR}/data/com.wbuarque.elementary-app.desktop DESTINATION ${DATADIR}/applications/)
install (FILES ${CMAKE_CURRENT_SOURCE_DIR}/data/com.wbuarque.elementary-app.appdata.xml DESTINATION ${DATADIR}/metainfo/)
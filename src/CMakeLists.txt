SET(VERSION_MAJOR 0)
SET(VERSION_MINOR 0)
SET(VERSION_PATCH 0)

INCLUDE_DIRECTORIES(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_BINARY_DIR}
    ${MPL_INCLUDE_DIRS}
    ${MX_INCLUDE_DIRS}
    ${ANERLEY_INCLUDE_DIRS}
#    ${CLUTTER_GTK_INCLUDE_DIRS}
#    ${GCONF_INCLUDE_DIRS}
#    ${CHAMPLAIN_INCLUDE_DIRS}
#    ${GEOCLUE_INCLUDE_DIRS}
    ${NM_INCLUDE_DIRS}
    )


SET (PROGRAMS gogoo-panel-people)

ADD_DEFINITIONS(-DLOCALEDIR="${LOCALEDIR}")
ADD_DEFINITIONS(-DMX_CACHE="${DATADIR}/mx.cache")
ADD_DEFINITIONS(-DTHEMEDIR="${PKG_DATADIR}/${PROGRAMS}/theme")
ADD_DEFINITIONS(-DGETTEXT_PACKAGE="${GETTEXT_PACKAGE}")
ADD_DEFINITIONS(-DDATADIR="${PKG_DATADIR}/${PROGRAMS}")
ADD_DEFINITIONS(-DWITH_ONLINE_NM)

SET(MARSHALS
        ${CMAKE_CURRENT_BINARY_DIR}/sw-marshals.h
        ${CMAKE_CURRENT_BINARY_DIR}/sw-marshals.c
)

########### Add marshal target ###############
ADD_CUSTOM_COMMAND(
   OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/sw-marshals.h
   COMMAND glib-genmarshal --prefix=sw_marshal --header ${CMAKE_SOURCE_DIR}/src/sw-marshals.list > ${CMAKE_CURRENT_BINARY_DIR}/sw-marshals.h
   )

ADD_CUSTOM_COMMAND(
   OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/sw-marshals.c
   COMMAND glib-genmarshal --prefix=sw_marshal --body ${CMAKE_SOURCE_DIR}/src/sw-marshals.list --body > ${CMAKE_CURRENT_BINARY_DIR}/sw-marshals.c
   DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/sw-marshals.h
   )

SET(PROGRAM_SRCS
    mnb-people-panel.c 
    mnb-people-panel.h 
    meego-panel-people.c 
    sw-online.c 
    sw-online.h 
    sw-marshals.list 
    )

SET(PROGRAM_HEADERS
    )

ADD_EXECUTABLE(${PROGRAMS} ${PROGRAM_SRCS} ${PROGRAM_HEADERS} ${MARSHALS})


   
TARGET_LINK_LIBRARIES(${PROGRAMS}
    ${MPL_LIBRARIES}
    ${ANERLEY_LIBRARIES}
#    ${GCONF_LIBRARIES}
#    ${GEOCLUE_LIBRARIES}
#    ${SW_LIBRARIES}
    ${NM_LIBRARIES}
#    ${CLUTTER_GTK_LIBRARIES}
#    ${GCONF_LIBRARIES}
    )

INSTALL(TARGETS ${PROGRAMS} RUNTIME DESTINATION ${PKG_LIBEXECDIR})

########### Add .service target ###############
CONFIGURE_FILE(
  "${CMAKE_CURRENT_SOURCE_DIR}/com.meego.UX.Shell.Panels.people.service.in"
  "${CMAKE_CURRENT_BINARY_DIR}/com.meego.UX.Shell.Panels.people.service"
  IMMEDIATE @ONLY)
INSTALL(FILES ${CMAKE_CURRENT_BINARY_DIR}/com.meego.UX.Shell.Panels.people.service DESTINATION share/dbus-1/services)

ADD_SUBDIRECTORY(theme)

########### Add .desktop target ###############
CONFIGURE_FILE(
  "${CMAKE_CURRENT_SOURCE_DIR}/gogoo-panel-people.desktop.in"
  "${CMAKE_CURRENT_BINARY_DIR}/gogoo-panel-people.desktop"
  IMMEDIATE @ONLY)
INSTALL(FILES ${CMAKE_CURRENT_BINARY_DIR}/gogoo-panel-people.desktop DESTINATION ${DATA_DIR}/panels)



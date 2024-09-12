cmake_minimum_required(VERSION 3.27)
project(vlcQt VERSION 1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(vlcQt INTERFACE)

target_include_directories(vlcQt
  INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include/vlcQt>
)

target_link_directories(vlcQt
  INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/lib>
  $<INSTALL_INTERFACE:lib/vlcQt>
)

target_link_libraries(vlcQt
  INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/lib/VLCQtCore.lib>
  $<INSTALL_INTERFACE:VLCQtCore.lib>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/lib/VLCQtWidgets.lib>
  $<INSTALL_INTERFACE:VLCQtWidgets.lib>
)

include(CMakePackageConfigHelpers)
configure_package_config_file(
  "${CMAKE_CURRENT_SOURCE_DIR}/vlcQtConfig.cmake.in"
  "${CMAKE_CURRENT_BINARY_DIR}/vlcQtConfig.cmake"
  INSTALL_DESTINATION share/vlcQt
)

install(TARGETS vlcQt
  EXPORT vlcQtTargets
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

install(DIRECTORY
  "${CMAKE_CURRENT_SOURCE_DIR}/include/VLCQtCore"
  "${CMAKE_CURRENT_SOURCE_DIR}/include/VLCQtWidgets"
  DESTINATION include/vlcQt
)

install(FILES
  "${CMAKE_CURRENT_SOURCE_DIR}/lib/VLCQtCore.lib"
  "${CMAKE_CURRENT_SOURCE_DIR}/lib/VLCQtWidgets.lib"
  DESTINATION lib/vlcQt
)

install(DIRECTORY
  "${CMAKE_CURRENT_SOURCE_DIR}/bin"
  DESTINATION /
)

install(
  EXPORT vlcQtTargets
  DESTINATION share/vlcQt
  NAMESPACE vlcQt::
)

install(
  FILES
  "${CMAKE_CURRENT_BINARY_DIR}/vlcQtConfig.cmake"
  DESTINATION share/vlcQt
)
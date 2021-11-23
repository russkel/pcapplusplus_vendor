cmake_minimum_required(VERSION 3.5)

project(pcapplusplus_vendor)

find_package(ament_cmake REQUIRED)

macro(unpack_pcpp)
  include(ExternalProject)
  file(MAKE_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/install)

  externalproject_add(pcapplusplus-release
    URL https://github.com/seladb/PcapPlusPlus/releases/download/v21.11/pcapplusplus-21.11-ubuntu-20.04-gcc-9.tar.gz
    URL_HASH SHA256=3f5f3ac8d078a518bccb2ace7199a2d9e1810122a369cf8762052b2a6d44532b
    DOWNLOAD_DIR ${CMAKE_CURRENT_SOURCE_DIR}
    BUILD_IN_SOURCE True
    SOURCE_DIR ${CMAKE_CURRENT_BINARY_DIR}/install/
    CONFIGURE_COMMAND ""
    BUILD_COMMAND ""
    INSTALL_COMMAND ""
  )

  install(
    DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/install/header/
    DESTINATION include
  )

  file(GLOB pcappp_libs "${CMAKE_CURRENT_BINARY_DIR}/install/*.a")

  install(
    FILES ${pcappp_libs}
    DESTINATION lib
  )

endmacro()

unpack_pcpp()

ament_export_include_directories(include)
ament_export_libraries(${pcappp_libs})
# this ensures that the package has an environment hook setting the PATH
ament_package()

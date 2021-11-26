cmake_minimum_required(VERSION 3.5)

project(pcapplusplus_vendor)

find_package(ament_cmake REQUIRED)

set(pcappp_libs
  libCommon++.a
  libPacket++.a
  libPcap++.a
)

macro(unpack_pcpp)
  include(ExternalProject)

  externalproject_add(pcapplusplus-release
    URL https://github.com/seladb/PcapPlusPlus/releases/download/v21.11/pcapplusplus-21.11-ubuntu-20.04-gcc-9.tar.gz
    URL_HASH SHA256=3f5f3ac8d078a518bccb2ace7199a2d9e1810122a369cf8762052b2a6d44532b
    DOWNLOAD_DIR ${CMAKE_CURRENT_SOURCE_DIR}
    BUILD_IN_SOURCE True
    SOURCE_DIR ${CMAKE_CURRENT_BINARY_DIR}/src/
    CONFIGURE_COMMAND ""
    BUILD_COMMAND ""
    INSTALL_COMMAND ""
  )

  install(
    DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/src/header/
    DESTINATION include
  )

  foreach(L ${pcappp_libs} )
    set(pcappp_lib_paths ${pcappp_lib_paths} ${CMAKE_CURRENT_BINARY_DIR}/src/${L})
  endforeach(L)

  install(
    FILES ${pcappp_lib_paths}
    DESTINATION lib
  )

endmacro()

unpack_pcpp()

ament_export_include_directories(include)
ament_export_libraries(${pcappp_libs})
# this ensures that the package has an environment hook setting the PATH
ament_package()

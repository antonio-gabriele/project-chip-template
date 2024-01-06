cmake_minimum_required(VERSION 3.19.0)

set(CHIP_ROOT /opt/homesecure-c)
set(CHIP_ROOT_OUT ${CHIP_ROOT}/out/custom)

project(BridgeApp)

find_package(PkgConfig REQUIRED)

pkg_check_modules(deps REQUIRED IMPORTED_TARGET glib-2.0 gobject-2.0 gio-2.0 openssl)

link_directories(${CHIP_ROOT_OUT}/lib)

file(GLOB SRCS
     "${CHIP_ROOT}/src/app/*.cpp"
)


add_executable(BridgeApp 
  main.cpp
  Device.cpp
  bridged-actions-stub.cpp
  ${CHIP_ROOT}/examples/platform/linux/AppMain.cpp
  ${CHIP_ROOT}/examples/platform/linux/CommissionableInit.cpp
  ${CHIP_ROOT}/examples/platform/linux/CommissionerMain.cpp
  ${CHIP_ROOT}/examples/platform/linux/LinuxCommissionableDataProvider.cpp
  ${CHIP_ROOT}/examples/platform/linux/NamedPipeCommands.cpp
  ${CHIP_ROOT}/examples/platform/linux/Options.cpp
  ${SRCS}
  )

target_compile_options(BridgeApp PRIVATE
  "-DCHIP_ADDRESS_RESOLVE_IMPL_INCLUDE_HEADER=<lib/address_resolve/AddressResolve_DefaultImpl.h>"
  "-DCHIP_SYSTEM_CONFIG_USE_SOCKETS"
  "-DCHIP_CONFIG_ENABLE_ARG_PARSER"
  "-DCHIP_DEVICE_CONFIG_MAX_DISCOVERED_IP_ADDRESSES=64"
  "-DCHIP_DEVICE_LAYER_TARGET=Linux"
  "-DINET_UDP_END_POINT_IMPL_CONFIG_FILE=<UDPEndPointImplSockets.h>"
  )

target_include_directories(BridgeApp PRIVATE
  ${CHIP_ROOT}/src
  ${CHIP_ROOT}/src/inet
  ${CHIP_ROOT}/src/include
  ${CHIP_ROOT}/examples/providers
  ${CHIP_ROOT}/src/platform/Linux
  ${CHIP_ROOT}/examples/platform/linux
  ${CHIP_ROOT}/third_party/nlio/repo/include
  ${CHIP_ROOT}/third_party/nlassert/repo/include
  ${CHIP_ROOT}/third_party/inipp/repo/inipp
  ${CHIP_ROOT}/zzz_generated/app-common
  ${CHIP_ROOT_OUT}/gen/src/controller/data_model/zapgen
  ${CHIP_ROOT_OUT}/gen/include
  ${CHIP_ROOT_OUT}/gen/examples/all-clusters-app/all-clusters-common
  )

target_link_libraries(BridgeApp 
  CHIP
  PkgConfig::deps
  )
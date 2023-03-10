include(CMakePackageConfigHelpers)

set(SRCS 
  bitstream.cc
  context.cc
  md5.cc
  nal-parser.cc
  nal.cc
  pps.cc
  refpic.cc
  scan.cc
  sei.cc
  sps.cc
  util.cc
  vps.cc
  vui.cc
)

add_definitions(-DLIBP265_EXPORTS)

add_library(libp265 ${SRCS})
target_link_libraries(libp265 PRIVATE Threads::Threads)
target_include_directories(libp265 PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/../include)

write_basic_package_version_file(libp265ConfigVersion.cmake COMPATIBILITY ExactVersion)

install(TARGETS libp265 EXPORT libp265Config
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
)

install(EXPORT libp265Config DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/libp265")

install(FILES ${CMAKE_CURRENT_BINARY_DIR}/libp265ConfigVersion.cmake DESTINATION
    "${CMAKE_INSTALL_LIBDIR}/cmake/libp265"
)

set(prefix ${CMAKE_INSTALL_PREFIX})
set(exec_prefix "\${prefix}")
if(IS_ABSOLUTE "${CMAKE_INSTALL_LIBDIR}")
    set(libdir "${CMAKE_INSTALL_LIBDIR}")
else()
    set(libdir "\${exec_prefix}/${CMAKE_INSTALL_LIBDIR}")
endif()
if(IS_ABSOLUTE "${CMAKE_INSTALL_INCLUDEDIR}")
    set(includedir "${CMAKE_INSTALL_INCLUDEDIR}")
else()
    set(includedir "\${prefix}/${CMAKE_INSTALL_INCLUDEDIR}")
endif()

configure_file(../libp265.pc.in ${CMAKE_CURRENT_BINARY_DIR}/libp265.pc @ONLY)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/libp265.pc DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig)
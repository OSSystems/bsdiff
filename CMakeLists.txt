cmake_minimum_required(VERSION 2.8.9)
project(bsdiff)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

include(GNUInstallDirs)

find_package(BZip2)

# bsdiff library
add_library(bsdiff-lib SHARED bsdiff.c)
add_library(bsdiff-staticlib STATIC bsdiff.c)
set_target_properties(bsdiff-lib PROPERTIES OUTPUT_NAME bsdiff)
set_target_properties(bsdiff-staticlib PROPERTIES OUTPUT_NAME bsdiff)
set_target_properties(bsdiff-staticlib PROPERTIES COMPILE_FLAGS -fPIC)
set_target_properties(bsdiff-lib PROPERTIES PUBLIC_HEADER bsdiff.h)

# bspatch library
add_library(bspatch-lib SHARED bspatch.c)
add_library(bspatch-staticlib STATIC bspatch.c)
set_target_properties(bspatch-lib PROPERTIES OUTPUT_NAME bspatch)
set_target_properties(bspatch-staticlib PROPERTIES OUTPUT_NAME bspatch)
set_target_properties(bspatch-staticlib PROPERTIES COMPILE_FLAGS -fPIC)
set_target_properties(bspatch-lib PROPERTIES PUBLIC_HEADER bspatch.h)

# bsdiff binary
add_executable(bsdiff-bin bsdiff.c)
target_link_libraries(bsdiff-bin ${BZIP2_LIBRARIES})
set_target_properties(bsdiff-bin PROPERTIES OUTPUT_NAME bsdiff)
target_compile_definitions(bsdiff-bin PRIVATE BSDIFF_EXECUTABLE=1)

# bspatch binary
add_executable(bspatch-bin bspatch.c)
target_link_libraries(bspatch-bin ${BZIP2_LIBRARIES})
set_target_properties(bspatch-bin PROPERTIES OUTPUT_NAME bspatch)
target_compile_definitions(bspatch-bin PRIVATE BSPATCH_EXECUTABLE=1)

# install targets
install(TARGETS bsdiff-lib bsdiff-staticlib bspatch-lib bspatch-staticlib
  LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}"
  ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}"
  PUBLIC_HEADER DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}")
install(TARGETS bsdiff-bin bspatch-bin DESTINATION "${CMAKE_INSTALL_BINDIR}")

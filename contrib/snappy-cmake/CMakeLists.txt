set (SOURCE_DIR "${CMAKE_SOURCE_DIR}/contrib/snappy")

set (SNAPPY_IS_BIG_ENDIAN 0)

set (HAVE_BYTESWAP_H 1)
set (HAVE_SYS_MMAN_H 1)
set (HAVE_SYS_RESOURCE_H 1)
set (HAVE_SYS_TIME_H 1)
set (HAVE_SYS_UIO_H 1)
set (HAVE_UNISTD_H 1)

set (HAVE_BUILTIN_EXPECT 1)
set (HAVE_BUILTIN_CTZ 1)
set (HAVE_FUNC_MMAP 1)
set (HAVE_FUNC_SYSCONF 1)

if (ARCH_AMD64 AND ENABLE_SSSE3)
    set (SNAPPY_HAVE_SSSE3 1)
else ()
    set (SNAPPY_HAVE_SSSE3 0)
endif ()

configure_file(
  "${SOURCE_DIR}/cmake/config.h.in"
  "${CMAKE_CURRENT_BINARY_DIR}/config.h")

set(HAVE_SYS_UIO_H_01 1)

configure_file(
  "${SOURCE_DIR}/snappy-stubs-public.h.in"
  "${CMAKE_CURRENT_BINARY_DIR}/snappy-stubs-public.h")

add_library(snappy "")
target_sources(snappy
  PRIVATE
    "${SOURCE_DIR}/snappy-internal.h"
    "${SOURCE_DIR}/snappy-stubs-internal.h"
    "${SOURCE_DIR}/snappy-c.cc"
    "${SOURCE_DIR}/snappy-sinksource.cc"
    "${SOURCE_DIR}/snappy-stubs-internal.cc"
    "${SOURCE_DIR}/snappy.cc")

target_include_directories(snappy PUBLIC ${SOURCE_DIR} ${CMAKE_CURRENT_BINARY_DIR})
target_compile_definitions(snappy PRIVATE -DHAVE_CONFIG_H)

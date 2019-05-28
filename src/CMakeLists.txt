# Copyright (c) Glyn Matthews 2012-2016.
# Distributed under the Boost Software License, Version 1.0.
# (See accompanying file LICENSE_1_0.txt or copy at
# http://www.boost.org/LICENSE_1_0.txt)

set(Uri_SRCS
  ${CMAKE_CURRENT_SOURCE_DIR}/uri.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/uri_builder.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/uri_errors.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/detail/uri_parse.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/detail/uri_parse_authority.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/detail/uri_advance_parts.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/detail/uri_normalize.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/detail/uri_resolve.cpp
  )

add_library(network-uri ${Uri_SRCS})
target_link_libraries(network-uri)
target_include_directories(network-uri
  PRIVATE
    ${Uri_SOURCE_DIR}/src
  PUBLIC
    $<BUILD_INTERFACE:${Uri_SOURCE_DIR}/include>
  INTERFACE
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_PREFIX}/include>
)
set_property(TARGET network-uri PROPERTY POSITION_INDEPENDENT_CODE ON)
if(${CMAKE_CXX_COMPILER_ID} MATCHES Clang)
  if (NOT Uri_DISABLE_LIBCXX)
    target_link_libraries(network-uri "c++")
  endif()
endif()

#propagate sources to parent scope for one-lib-build
set(Uri_SRCS ${Uri_SRCS} PARENT_SCOPE)

install(
  TARGETS network-uri
  EXPORT cpp-netlib-uriConfig
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
)

if (Uri_EXPORT_TARGET)
  install(
    EXPORT cpp-netlib-uriConfig
    DESTINATION lib/cmake/cpp-netlib-uri
    NAMESPACE cpp-netlib::
  )
endif()

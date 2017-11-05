# source: https://github.com/tensorflow/tensorflow/blob/master/tensorflow/contrib/cmake/patches/highwayhash/CMakeLists.txt

cmake_minimum_required(VERSION 3.0.0)

project(highwayhash VERSION 0.0.0) # no version specified in repo

set(HIGHWAYHASH_SRCS
    "highwayhash/code_annotation.h"
    "highwayhash/highway_tree_hash.cc"
    "highwayhash/highway_tree_hash.h"
    "highwayhash/scalar_highway_tree_hash.cc"
    "highwayhash/scalar_highway_tree_hash.h"
    "highwayhash/scalar_sip_tree_hash.cc"
    "highwayhash/scalar_sip_tree_hash.h"
    "highwayhash/sip_hash.cc"
    "highwayhash/sip_hash.h"
    "highwayhash/sip_tree_hash.cc"
    "highwayhash/sip_tree_hash.h"
    "highwayhash/sse41_highway_tree_hash.cc"
    "highwayhash/sse41_highway_tree_hash.h"
    "highwayhash/state_helpers.h"
    "highwayhash/types.h"
    "highwayhash/vec.h"
    "highwayhash/vec2.h"
)

set(HIGHWAYHASH_INCLUDES
    "highwayhash/code_annotation.h"
    "highwayhash/highway_tree_hash.h"
    "highwayhash/scalar_highway_tree_hash.h"
    "highwayhash/scalar_sip_tree_hash.h"
    "highwayhash/sip_hash.h"
    "highwayhash/sip_tree_hash.h"
    "highwayhash/sse41_highway_tree_hash.h"
    "highwayhash/state_helpers.h"
    "highwayhash/types.h"
    "highwayhash/vec.h"
    "highwayhash/vec2.h"
)

add_library(highwayhash ${HIGHWAYHASH_SRCS})

target_include_directories(highwayhash
  PUBLIC
  "$<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}>"
)

# C++11
target_compile_features(highwayhash PRIVATE
    cxx_rvalue_references
)

### Install ###
set(generated_dir "${CMAKE_CURRENT_BINARY_DIR}/generated")

set(config_install_dir "lib/cmake/${PROJECT_NAME}")
set(include_install_dir "include")

set(version_config "${generated_dir}/${PROJECT_NAME}ConfigVersion.cmake")
set(project_config "${generated_dir}/${PROJECT_NAME}Config.cmake")
set(targets_export_name "${PROJECT_NAME}Targets")
set(namespace "${PROJECT_NAME}::")

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
  "${version_config}" COMPATIBILITY SameMajorVersion
  )

# Note: use 'targets_export_name'
configure_package_config_file(
  "${PROJECT_SOURCE_DIR}/cmake/Config.cmake.in"
  "${project_config}"
  INSTALL_DESTINATION "${config_install_dir}"
  )

install(
  TARGETS ${PROJECT_NAME}
  EXPORT "${targets_export_name}"
  LIBRARY DESTINATION "lib"
  ARCHIVE DESTINATION "lib"
  RUNTIME DESTINATION "bin"
  INCLUDES DESTINATION "${include_install_dir}"
  )

install(
  DIRECTORY "highwayhash" # No trailing slash
  DESTINATION "${include_install_dir}"
  FILES_MATCHING PATTERN "*.h"
  )

install(
  FILES "${project_config}" "${version_config}"
  DESTINATION "${config_install_dir}"
  )

install(
  EXPORT "${targets_export_name}"
  NAMESPACE "${namespace}"
  DESTINATION "${config_install_dir}"
  )
  


cmake_minimum_required(VERSION 3.12)

project(tsl_hat_trie_project)

add_library(tsl_hat_trie INTERFACE)
# Use tsl::hat_trie as target, more consistent with other libraries conventions (Boost, Qt, ...)
add_library(tsl::hat_trie ALIAS tsl_hat_trie)

target_include_directories(tsl_hat_trie
    INTERFACE $<BUILD_INTERFACE:"${CMAKE_CURRENT_SOURCE_DIR}/include">
    )

target_sources(tsl_hat_trie INTERFACE $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/tsl/array-hash/array_growth_policy.h>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/tsl/array-hash/array_hash.h>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/tsl/array-hash/array_map.h>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/tsl/array-hash/array_set.h>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/tsl/htrie_hash.h>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/tsl/htrie_map.h>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/tsl/htrie_set.h>)


if (${CMAKE_VERSION} VERSION_GREATER "3.7")
  # Only available since version 3.8
  target_compile_features(tsl_hat_trie INTERFACE cxx_std_11)
endif ()


include(GNUInstallDirs)

function(install_target targets config)
  install(TARGETS ${targets} EXPORT ${config}
      LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
      ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
      RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
      INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
      PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
      FRAMEWORK DESTINATION ${CMAKE_INSTALL_PREFIX}
      )
endfunction()

install_target(tsl_hat_trie tsl_hat_trieConfig)

install(
    DIRECTORY ${CMAKE_SOURCE_DIR}/include/tsl
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)
install(
    EXPORT tsl_hat_trieConfig
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake
    NAMESPACE tsl::
)

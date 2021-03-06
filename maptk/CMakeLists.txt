include(CheckCXXCompilerFlag)

include_directories("${MAPTK_SOURCE_DIR}")
include_directories("${MAPTK_BINARY_DIR}")


###
# Setting up main library
#
set(maptk_public_headers
  epipolar_geometry.h
  metrics.h
  local_geo_cs.h
  projected_track_set.h
  geo_reference_points_io.h
  ins_data.h
  ins_data_io.h
  interpolate_camera.h
  match_matrix.h
  triangulate.h
  transform.h

  plugin_interface/algorithm_plugin_interface.h
  plugin_interface/algorithm_plugin_interface_macros.h
  )


set(maptk_sources
  colorize.cxx
  epipolar_geometry.cxx
  geo_reference_points_io.cxx
  ins_data.cxx
  ins_data_io.cxx
  interpolate_camera.cxx
  local_geo_cs.cxx
  match_matrix.cxx
  metrics.cxx
  projected_track_set.cxx
  transform.cxx
  triangulate.cxx
  )

kwiver_configure_file( version.h
  "${CMAKE_CURRENT_SOURCE_DIR}/version.h.in"
  "${CMAKE_CURRENT_BINARY_DIR}/version.h"
  MAPTK_VERSION
  MAPTK_COPYRIGHT_YEAR
)

# Address MSVC fatal error C1128:
# number of sections exceeded object file format limit
if(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
  set_source_files_properties(
    triangulate.cxx
    PROPERTIES COMPILE_FLAGS "/bigobj"
    )
endif()

kwiver_install_headers(
  SUBDIR        maptk
  ${maptk_public_headers}
  )
kwiver_add_library(
  maptk
  ${maptk_public_headers}
  ${maptk_sources}
  )

target_link_libraries( maptk
  PUBLIC               vital
                       kwiversys
  )


###
#Cconfigure plugins
#
add_subdirectory(plugins)


# Configuring/Adding compile definitions to target
# (so we can use generator expressions)

if (CMAKE_CONFIGURATION_TYPES)
  set(config_subdir "/$<CONFIGURATION>")
endif()

###
# Marking source root for documentation generation
#
kwiver_create_doxygen( maptk "${CMAKE_CURRENT_LIST_DIR}" )

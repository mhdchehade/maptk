# Build / Install plugin containing OpenCV algorithm implementations + support
# structures

set( maptk_ocv_headers_public
  analyze_tracks.h
  descriptor_set.h
  detect_features.h
  detect_features_AGAST.h
  detect_features_FAST.h
  detect_features_GFTT.h
  detect_features_MSD.h
  detect_features_MSER.h
  detect_features_simple_blob.h
  detect_features_STAR.h
  draw_tracks.h
  estimate_fundamental_matrix.h
  estimate_homography.h
  extract_descriptors.h
  extract_descriptors_BRIEF.h
  extract_descriptors_DAISY.h
  extract_descriptors_FREAK.h
  extract_descriptors_LATCH.h
  extract_descriptors_LUCID.h
  feature_set.h
  feature_detect_extract_BRISK.h
  feature_detect_extract_ORB.h
  feature_detect_extract_SIFT.h
  feature_detect_extract_SURF.h
  image_container.h
  image_io.h
  mat_image_memory.h
  match_features.h
  match_features_bruteforce.h
  match_features_flannbased.h
  match_set.h
  register_algorithms.h
  )
kwiver_install_headers(
  SUBDIR     maptk/plugins/ocv
  ${maptk_ocv_headers_public}
  )
kwiver_install_headers(
  ${CMAKE_CURRENT_BINARY_DIR}/maptk_ocv_export.h
  NOPATH   SUBDIR     maptk/plugins/ocv
  )

set( maptk_ocv_sources
  analyze_tracks.cxx
  descriptor_set.cxx
  detect_features.cxx
  detect_features_AGAST.cxx
  detect_features_FAST.cxx
  detect_features_GFTT.cxx
  detect_features_MSD.cxx
  detect_features_MSER.cxx
  detect_features_simple_blob.cxx
  detect_features_STAR.cxx
  draw_tracks.cxx
  estimate_fundamental_matrix.cxx
  estimate_homography.cxx
  extract_descriptors.cxx
  extract_descriptors_BRIEF.cxx
  extract_descriptors_DAISY.cxx
  extract_descriptors_FREAK.cxx
  extract_descriptors_LATCH.cxx
  extract_descriptors_LUCID.cxx
  feature_set.cxx
  feature_detect_extract_BRISK.cxx
  feature_detect_extract_ORB.cxx
  feature_detect_extract_SIFT.cxx
  feature_detect_extract_SURF.cxx
  image_container.cxx
  image_io.cxx
  mat_image_memory.cxx
  match_features.cxx
  match_features_bruteforce.cxx
  match_features_flannbased.cxx
  match_set.cxx
  register_algorithms.cxx
  )

if( OpenCV_FOUND AND
    (OpenCV_VERSION VERSION_GREATER "3.0" OR
     OpenCV_VERSION VERSION_EQUAL "3.0") )
  message(STATUS "Found OPENCV 3.0")
  add_definitions(-DMAPTK_HAS_OPENCV_VER_3)
endif()

kwiver_add_library( maptk_ocv
  ${maptk_ocv_headers_public}
  ${maptk_ocv_sources}
  )
target_link_libraries( maptk_ocv
  PUBLIC               maptk
                     ${OpenCV_LIBS}
  )

maptk_create_plugin( maptk_ocv
  algorithm_plugin_interface.cxx
  )

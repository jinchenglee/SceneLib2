############################################################
## Required Dependencies
FIND_PACKAGE(Boost COMPONENTS thread filesystem system REQUIRED QUIET)
FIND_PACKAGE(Eigen3 REQUIRED)
FIND_PACKAGE(GLUT REQUIRED)
FIND_PACKAGE(OpenCV REQUIRED)
FIND_PACKAGE(Pangolin REQUIRED)

SET(OPENCV_LIBS     /usr/local/lib/libopencv_aruco.so
    /usr/local/lib/libopencv_bgsegm.so
    /usr/local/lib/libopencv_bioinspired.so
    /usr/local/lib/libopencv_calib3d.so
    /usr/local/lib/libopencv_ccalib.so
    /usr/local/lib/libopencv_core.so
    /usr/local/lib/libopencv_cudaarithm.so
    /usr/local/lib/libopencv_cudabgsegm.so
    /usr/local/lib/libopencv_cudacodec.so
    /usr/local/lib/libopencv_cudafeatures2d.so
    /usr/local/lib/libopencv_cudafilters.so
    /usr/local/lib/libopencv_cudaimgproc.so
    /usr/local/lib/libopencv_cudalegacy.so
    /usr/local/lib/libopencv_cudaobjdetect.so
    /usr/local/lib/libopencv_cudaoptflow.so
    /usr/local/lib/libopencv_cudastereo.so
    /usr/local/lib/libopencv_cudawarping.so
    /usr/local/lib/libopencv_cudev.so
    /usr/local/lib/libopencv_datasets.so
    /usr/local/lib/libopencv_dnn.so
    /usr/local/lib/libopencv_dpm.so
    /usr/local/lib/libopencv_face.so
    /usr/local/lib/libopencv_features2d.so
    /usr/local/lib/libopencv_flann.so
    /usr/local/lib/libopencv_fuzzy.so
    /usr/local/lib/libopencv_hdf.so
    /usr/local/lib/libopencv_highgui.so
    /usr/local/lib/libopencv_imgcodecs.so
    /usr/local/lib/libopencv_imgproc.so
    /usr/local/lib/libopencv_line_descriptor.so
    /usr/local/lib/libopencv_ml.so
    /usr/local/lib/libopencv_objdetect.so
    /usr/local/lib/libopencv_optflow.so
    /usr/local/lib/libopencv_photo.so
    /usr/local/lib/libopencv_plot.so
    /usr/local/lib/libopencv_reg.so
    /usr/local/lib/libopencv_rgbd.so
    /usr/local/lib/libopencv_saliency.so
    /usr/local/lib/libopencv_shape.so
    /usr/local/lib/libopencv_stereo.so
    /usr/local/lib/libopencv_stitching.so
    /usr/local/lib/libopencv_structured_light.so
    /usr/local/lib/libopencv_superres.so
    /usr/local/lib/libopencv_surface_matching.so
    /usr/local/lib/libopencv_text.so
    /usr/local/lib/libopencv_tracking.so
    /usr/local/lib/libopencv_videoio.so
    /usr/local/lib/libopencv_video.so
    /usr/local/lib/libopencv_videostab.so
    /usr/local/lib/libopencv_viz.so
    /usr/local/lib/libopencv_xfeatures2d.so
    /usr/local/lib/libopencv_ximgproc.so
    /usr/local/lib/libopencv_xobjdetect.so
    /usr/local/lib/libopencv_xphoto.so
    )

# Apply required dependencies settings
LIST(APPEND USER_INC "${Boost_INCLUDE_DIRS};${Pangolin_INCLUDE_DIRS};${CMAKE_CURRENT_SOURCE_DIR};${CMAKE_CURRENT_BINARY_DIR};${EIGEN3_INCLUDE_DIR};${OpenCV_INCLUDE_DIRS}")
LIST(APPEND LIB_DIRS "${Boost_LIBRARY_DIRS};${Pangolin_LIBRARY_DIRS};${PROJECT_BINARY_DIR}/src;${OpenCV_LIB_DIR}")
LIST(APPEND LINK_LIBS "${OPENCV_LIBS};${Boost_LIBRARIES};${GLUT_LIBRARIES};${Pangolin_LIBRARIES};${OpenCV_LIBS}")

############################################################
## Add Libraries / Include Directories / Link Directories
INCLUDE_DIRECTORIES(${INTERNAL_INC})
INCLUDE_DIRECTORIES(${USER_INC})
LINK_DIRECTORIES(${LIB_DIRS})
LINK_LIBRARIES(${LINK_LIBS})

############################################################
## SceneLib2 Library Sources
SET(SOURCES monoslam.h monoslam.cpp
            motion_model.h motion_model.cpp
            feature.h feature.cpp feature_init_info.h feature_init_info.cpp
            feature_model.h feature_model.cpp
            part_feature_model.h part_feature_model.cpp
            full_feature_model.h full_feature_model.cpp
            camera.h camera.cpp kalman.h kalman.cpp
            framegrabber/framegrabber.h framegrabber/framegrabber.cpp
            framegrabber/filegrabber.h framegrabber/filegrabber.cpp
            framegrabber/usbcamgrabber.h framegrabber/usbcamgrabber.cpp
            graphic/graphictool.h graphic/graphictool.cpp
            support/math_util.h support/math_util.cpp
            support/eigen_util.h support/eigen_util.cpp
            support/pangolin_util.h support/pangolin_util.cpp
            improc/improc.h improc/improc.cpp
            improc/search_multiple_overlapping_ellipses.h
            improc/search_multiple_overlapping_ellipses.cpp)

ADD_LIBRARY(${LIBRARY_NAME} SHARED ${SOURCES})

############################################################
## Export config file for easy inclusion into other projects: FIND_PACKAGE(SceneLib2)
CONFIGURE_FILE  (
                ${CMAKE_CURRENT_SOURCE_DIR}/SceneLib2Config.cmake.in
                ${CMAKE_CURRENT_BINARY_DIR}/SceneLib2Config.cmake
                @ONLY IMMEDIATE
                )

EXPORT(PACKAGE SceneLib2)

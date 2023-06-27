cmake_minimum_required(VERSION 3.14 FATAL_ERROR)

project(
    TMIV
    DESCRIPTION "Test Model for MPEG Immersive Video"
    VERSION 8.0.2
    LANGUAGES C CXX)

set(CMAKE_CXX_EXTENSIONS OFF)

option(NO_INTERNET "Use pre-downloaded source archives for external libraries, e.g. Catch2, HM, and fmt" OFF)

include(cmake/fmt.cmake)

include(cmake/catch2.cmake)

if (BUILD_CATCH2)
    include(CTest)
    include(Catch)
    enable_testing()
endif()

include(GNUInstallDirs)

set_property(GLOBAL PROPERTY USE_FOLDERS ON)
include(cmake/tmiv_module_creation.cmake)
include(cmake/test_creation.cmake)

set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package(Threads REQUIRED)

include(cmake/HM.cmake)
include(cmake/VVenC.cmake)

# Optionally enable clang-tidy, but only for TMIV modules
include(cmake/clang_tidy.cmake)

foreach(subdir
        Common 
        MivBitstream 
        IO
        ViewOptimizer
        Pruner
        Aggregator
        Packer
        GeometryQuantizer
        Renderer
        ViewingSpace        
        VideoDecoder
        Encoder
        Decoder
        Parser
        DepthQualityAssessor)
    add_subdirectory(source/${subdir})
endforeach()

add_custom_target(clang_format
    COMMAND
        ${CMAKE_COMMAND}
        "-DANALYZE_DIR=${CMAKE_SOURCE_DIR}/source"
        "-P"
        "${CMAKE_CURRENT_LIST_DIR}/cmake/clang_format.cmake")
set_property(TARGET clang_format PROPERTY FOLDER "Utilities")

add_custom_target(module_graph
    COMMAND
        ${CMAKE_COMMAND}
        "-DOUTPUT_FILE=${CMAKE_SOURCE_DIR}/doc/module_graph.svg"
        "-P"
        "${CMAKE_CURRENT_LIST_DIR}/cmake/module_graph.cmake")
set_property(TARGET module_graph PROPERTY FOLDER "Utilities")

include(cmake/integration_test.cmake)

# To use TMIV as a sub project, clone, build and install.
# Then in the CMake GUI set TMIV_DIR to the lib/cmake/TMIV directory of the TMIV installation.
# In the super project CMakeLists.txt use find_package(TMIV 8.0.2 REQUIRED)
set(TMIV_CMAKE_CONFIG_DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/TMIV")

install(
    EXPORT TMIVTargets
    FILE "TMIVTargets.cmake"
    NAMESPACE TMIV::
    DESTINATION ${TMIV_CMAKE_CONFIG_DESTINATION})
export(EXPORT TMIVTargets NAMESPACE TMIV::)

include(CMakePackageConfigHelpers)

configure_package_config_file(
    "${CMAKE_CURRENT_LIST_DIR}/cmake/TMIVConfig.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/TMIVConfig.cmake"
    INSTALL_DESTINATION ${TMIV_CMAKE_CONFIG_DESTINATION})

write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/TMIVConfigVersion.cmake"
    VERSION ${TMIV_VERSION}
    COMPATIBILITY SameMajorVersion)

install(
    FILES
        "${CMAKE_CURRENT_BINARY_DIR}/TMIVConfig.cmake"
        "${CMAKE_CURRENT_BINARY_DIR}/TMIVConfigVersion.cmake"
    DESTINATION ${TMIV_CMAKE_CONFIG_DESTINATION})

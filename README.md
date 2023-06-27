Test Model for MPEG Immersive Video (TMIV)
==========================================

[![pipeline status](http://mpegx.int-evry.fr/software/MPEG/MIV/RS/TM1/badges/v8.0-dev/pipeline.svg)](http://mpegx.int-evry.fr/software/MPEG/MIV/RS/TM1/-/commits/v8.0-dev)
[![coverage report](http://mpegx.int-evry.fr/software/MPEG/MIV/RS/TM1/badges/v8.0-dev/coverage.svg)](http://mpegx.int-evry.fr/software/MPEG/MIV/RS/TM1/-/graphs/v8.0-dev/charts)

1. [Introduction](#introduction)
1. [Scope](#scope)
1. [Build and installation instructions](#build-and-installation-instructions)
   1. [Prerequisites to follow this instruction](#prerequisites-to-follow-this-instruction)
   1. [Obtain a version of TMIV](#obtain-a-version-of-tmiv)
   1. [Preparing for TMIV build on a machine without internet connection](#preparing-for-tmiv-build-on-a-machine-without-internet-connection)
   1. [Building and installing TMIV](#building-and-installing-tmiv)
   1. [Installation result](#installation-result)
1. [Running the software tests](#running-the-software-tests)
   1. [Unit tests](#unit-tests)
   1. [Integration tests](#integration-tests)
1. [Instructions to run TMIV](#instructions-to-run-tmiv)
   1. [Instructions for CTC conditions](#instructions-for-ctc-conditions)
   1. [Instructions for in-band HEVC video coding with HM](#instructions-for-in-band-hevc-video-coding-with-hm)
   1. [Instructions for multiplane image coding](#instructions-for-multiplane-image-coding)
1. [Overview of TMIV configuration files](#overview-of-tmiv-configuration-files)
   1. [Basic structure of the TMIV configuration files](#basic-structure-of-the-tmiv-configuration-files)
   1. [Input/output parameters](#inputoutput-parameters)
   1. [Algorithmic parameters](#algorithmic-parameters)
1. [Instruction to use TMIV as a library](#instruction-to-use-tmiv-as-a-library)
1. [Structure of the test model](#structure-of-the-test-model)
1. [Contributing to the test model](#contributing-to-the-test-model)
1. [References](#references)

# Introduction

This software project (TMIV-SW) is the ISO/IEC 23090-12 *MPEG Immersive Video* (MIV) reference software and is part of the *Test model for Immersive Video* [[4]](#references).

The MPEG-I project (ISO/IEC 23090) on *Coded Representation of Immersive Media* includes Part 2 *Omnidirectional MediA Format* (OMAF) version 1 published in 2018 that supports 3 Degrees of Freedom (3DoF), where a user’s position is static but its head can yaw, pitch and roll.
However, rendering flat 360° video, i.e. supporting head rotations only, may generate visual discomfort especially when objects close to the viewer are rendered. 6DoF enables translation movements in horizontal, vertical, and depth directions in addition to 3DoF orientations. The
translation support enables interactive motion parallax providing viewers with natural cues to their visual system and resulting in an enhanced perception of volume around them. At the 125th MPEG meeting, a call for proposals [[1]](#references) was issued to enable head-scale movements within a limited space. This has resulted in new MPEG-I Part 12 Immersive Video (MIV).

At the 129th MPEG meeting the fourth working draft of MIV has been realigned to use MPEG-I Part 5 *Video-based Point Cloud Compression* (V-PCC) as a normative reference for terms, defintions, syntax, semantics and decoding processes. At the 130th MPEG meeting this alignment has been completed by restructuring Part 5 in a common specification *Visual Volumetric Video-based Coding* (V3C) and annex H Video-based Point Cloud Compression (V-PCC). V3C provides extension mechanisms for V-PCC and MIV. The terminology in the test model reflects that of V3C and MIV.

# Scope

The normative decoding process for MPEG Immersive Video (MIV) is specified in *Text of ISO/IEC DIS 23090-12 MPEG Immersive Video* [[3]](#references). The latest public draft of the specification is *Potential improvements of MIV* [[2]](#references). The TMIV reference software provides a reference implementation of non-normative encoding and rendering techniques and the normative decoding process for the MIV
standard.

The test model document [[4]](#references) provides an algorithmic description for the TMIV encoder and decoder/renderer. The purpose of this document is to promote a common understanding of the coding features, in order to facilitate the assessment of the technical impact of new technologies during
the standardization process. *Common Test Conditions for MPEG Immersive Video* [[5]](#references) provides test conditions including TMIV-based anchors. Configuration files are included with the reference software.

# Build and installation instructions

Directory names in this instruction are merely examples.

## Prerequisites to follow this instruction

These instructions are valid for Windows and Linux.

The software is ISO C++17 conformant and requires the following external libraries:

* The [{fmt}](https://github.com/fmtlib/fmt) library for string formatting
* The [Catch2](https://github.com/catchorg/Catch2.git) test framework
* The [HEVC test model](https://vcgit.hhi.fraunhofer.de/jct-vc/HM.git) (HM)

Minimal prerequisites are:

* C++17 compiler and build tools
* CMake 3.14 or newer

The preferred method is to let the TMIV build configuration process download the correct versions of all the external libraries for you. This adds the following prerequisites:
* Git command-line tools
* Internet connection

For the fallback procedure, please continue reading at [Preparing for TMIV build on a machine without internet connection](#preparing-for-tmiv-build-on-a-machine-without-internet-connection).

## Obtain a version of TMIV

To obtain the latest public release of TMIV:

```shell
cd /Workspace
git clone https://gitlab.com/mpeg-i-visual/tmiv.git
```

To obtain the latest internal release of TMIV instead:

```shell
cd /Workspace
git clone http://mpegx.int-evry.fr/software/MPEG/MIV/RS/TM1.git tmiv
```

The default branch of the internal repository is the current development branch, e.g. `v7.0-dev`.

To switch on either server to a specific release of TMIV, e.g. 6.1, also execute the following command:

```shell
cd /Workspace/tmiv
git checkout v6.1
```

Please continue reading at [Building and installing TMIV](#building-and-installing-tmiv).

## Preparing for TMIV build on a machine without internet connection

The first step is to obtain source archives for TMIV and all external libraries on a machine with internet access.

### Catch2 testing framework

1. Visit this URL: https://github.com/catchorg/Catch2/tree/v2.11.1
1. Click on the green button "Code" and select "Download ZIP"
1. Unzip, resulting in a directory `/Workspace/Catch2-2.11.1` such that the file `/Workspace/Catch2-2.11.1/README.md` exists.

### {fmt} string formatting library

1. Visit this URL: https://github.com/fmtlib/fmt/tree/7.0.3
1. Click on the green button "Code" and select "Download ZIP"
1. Unzip, resulting in a directory `/Workspace/fmt-7.0.3` such that the file `/Workspace/fmt-7.0.3/README.md` exists.

### HEVC test model (HM)

1. Visit this URL: https://vcgit.hhi.fraunhofer.de/jct-vc/HM/-/tree/HM-16.16
1. Click on the download button (next to the Clone button) and select "zip"
1. Unzip, resulting in a directory `/Workspace/HM-HM-16.16` such that the file `/Workspace/HM-HM-16.16/README` exists.
1. Rename that directory to `/Workspace/HM-16.16`.

### Fraunhofer Versatile Video Encoder (VVenC)

1. Visit this URL: https://github.com/fraunhoferhhi/vvenc/tree/v0.2.0.0
1. Click on the green "Code" button and select "Download ZIP"
1. Unzip, resulting in a directory `/Workspace/vvenc-0.2.0.0` such that the file `/Workspace/vvenc-0.2.0.0/README.md` exists.

### Fraunhofer Versatile Video Decoder (VVdeC)

1. Visit this URL: https://github.com/fraunhoferhhi/vvdec/tree/v0.2.0.0
1. Click on the green "Code" button and select "Download ZIP"
1. Unzip, resulting in a directory `/Workspace/vvdec-0.2.0.0` such that the file `/Workspace/vvdec-0.2.0.0/README.md` exists.

### This project

1. For the latest public release, visit this URL: https://gitlab.com/mpeg-i-visual/tmiv
1. Click on the download button (next to the Clone button) and select "zip"
1. Unzip, resulting in a directory `/Workspace/tmiv-master` such that the file `/Workspace/tmiv-master/README.md` exists.
1. Rename to `/Workspace/tmiv` to match with the following instructions.

To select a specific release instead, e.g. 6.1, use this procedure instead:

1. Visit this URL: https://gitlab.com/mpeg-i-visual/tmiv/-/tree/v6.1 
1. Click on the download button (next to the Clone button) and select "zip"
1. Unzip, resulting in a directory `/Workspace/tmiv-v6.1` such that the file `/Workspace/tmiv-v6.1/README.md` exists.
1. Rename to `/Workspace/tmiv` to match with the following instructions.

The same instruction also works with the internal software repository. Pleae be aware, that the default branch of the internal repository is not `master` but the current development branch, e.g. `v7.0-dev`. This is not a stable version of TMIV.

In the next step add `-DNO_INTERNET=ON` to the first cmake command or set `NO_INTERNET` to `ON` in the CMake GUI. If the external libraries are placed in the right directories, then they are detected. Otherwise the location of each of the external libraries can be specified, for instance by entering the values in the CMake GUI.

## Building and installing TMIV

Below are two alternative instructions for building: using only command-line tools or using a GUI. Both instructions are compatible with Linux and Windows. Make sure to use forward slashes with CMake, also on Windows.

### Using the command line

To build and install TMIV into the directory `/Workspace/tmiv_install` with sources in the directory `/Workspace/tmiv`, execute:

```shell
mkdir /Workspace/tmiv_build
cd /Workspace/tmiv_build
cmake -DCMAKE_INSTALL_PREFIX=/Workspace/tmiv_install -DCMAKE_BUILD_TYPE=Release /Workspace/tmiv
cmake --build . --parallel --config Release
cmake --build . --parallel --config Release --target install
```

The intermediate directory `/Workspace/tmiv_build` may be deleted when the installation has completed.

### Using a GUI

Open the CMake GUI and specify:

* Where the source directory is: `/Workspace/tmiv`
* Where to build the binaries: `/Workspace/tmiv_build`
* Click Configure, Yes, Finish
* Do not change any flags starting with `CATCH_`
* Set `CMAKE_INSTALL_PREFIX` to `/Workspace/tmiv_install`
* Set `CMAKE_BUILD_TYPE` to `Release` (no need for Visual Studio)
* Click Generate

Build and install the generated project.

For Visual Studio please:
* Manually select `Release` from the drop-down box.
* Perform build by building the `ALL_BUILD` target.
* Perform installation by building the `INSTALL` target.

## Installation result

After installation, the TMIV executables `Encoder`, `Decoder` and `Renderer` will be available under the directory `/Workspace/tmiv_install/bin`.
By default TMIV only builds the HM modules that are required for TMIV (`TLibCommon` and `TLibDecoder`).
When `HM_BUILD_TAPPDECODER` and `HM_BUILD_TAPPENCODER` are selected, then the `TAppDecoder` and `TAppEncoder` tools respectively will also be installed to this directory.
TMIV per default builds `vvencFFapp` and `vvdecapp` from the Fraunhofer VVC implementations.

# Running the software tests

In general and especially when [contributing to TMIV](doc/CONTRIBUTING.md) it is a good idea to run the software tests before starting any (large) experiments. TMIV has two types of software tests that are integrated into the project.

## Unit tests

The unit tests check aspects of the software in isolation and without I/O. Running the 100+ unit tests only takes a couple of seconds in total.

To run all unit tests, execute the following command inside your build directory:

```shell
cmake --build . --parallel --config Release --target test
```

For Visual Studio please substitute `test` for `RUN_TESTS`.

## Integration tests

The integration test runs the TMIV executables on real data to make sure that there are no runtime errors in a range of conditions, and it reports if any bitstreams or YUV files have changed compared to the reference (when provided).

To run the integration test the following additional CMake variables are relevant:

* `INTEGRATION_TEST_CONTENT_DIR` needs to be set to `/Content` (see [Running the TMIV encoder](#running-the-tmiv-encoder)).
* `INTEGRATION_TEST_MAX_WORKERS` may be optionally set to limit the number of parallel processes.
* `INTEGRATION_TEST_OUTPUT_DIR` defaults to `/tmiv_build/integration_test` but can be changed.
* `INTEGRATION_TEST_REFERENCE_DIR`:
   * is left empty to create the reference (typically of the development branch, e.g. `vx.y-dev`)
   * is set equal to `INTEGRATION_TEST_OUTPUT_DIR` of that reference to compare against it.

To run the test execute the following command inside your build directory:

```shell
cmake --build . --parallel --config Release --target integration_test
```

# Instructions to run TMIV

Configuration files for CTC conditions are available under [config/ctc/](/config/ctc):

* *best_reference* renders from all source views without coding
* *miv_anchor* is the MIV anchor with patches
* *miv_view_anchor* is the MIV view anchor which codes a subset of views completely
* *miv_dsde_anchor* is the MIV decoder-side depth estimating anchor

In addition, there are other configurations available under [config/test/](/config/test) to illustrate different aspects of TMIV such as entity-based coding or multi-plane image (MPI) coding.

The file names of the configuration files, and the file names within them are only examples.

## Instructions for CTC conditions

It is assumed that the reader has read the CTC document [[5]](#references) first. This description does not replace that document.

Use the following steps to encode a bitstream and render a viewport:

* MIV anchor and MIV view anchor:
  1. Run the TMIV encoder
  1. Run the VVenC encoder on all video sub bitstreams
  1. Run the VVdeC decoder on all video sub bitstreams
  1. Run the TMIV decoder to decode the MIV bitstream to render a viewport
* MIV decoder-side depth estimating anchor:
  1. Run the TMIV encoder
  1. Run the VVenC encoder on all video sub bitstreams
  1. Run the VVdeC decoder on all video sub bitstreams
  1. Run the TMIV decoder only to decode the bitstream
  1. Run the Immersive Video Depth Estimator (IVDE)
  1. Run the TMIV renderer to render a viewport
* Best reference:
  1. Run the TMIV renderer to render a viewport

### Running the TMIV encoder

For this example, we will be using the MIV anchor [A_1_TMIV_encode.json](/config/ctc/miv_anchor/A_1_TMIV_encode.json) configuration and [A.json](/config/ctc/sequences/A.json) sequence configuration.

1. Place the color and depth videos [[5]](#references) in a folder arbitrarily named `/Content` in this description.
   * Your organization or one of the maintainers of this repository may be able to provide the test sequences to you.
   * Make sure to comply to the naming scheme defined in `inputTexturePathFmt` and `inputGeometryPathFmt`.
   * For example, when `inputDirectory` is equal to `/Content`,
   * and given that `inputGeometryPathFmt` is equal to `{1}/{3}_depth_{4}x{5}_{6}.yuv`,
   * then for content ID `A` (ClassroomVideo),
   * a source view `v3` from the set of source views,
   * a video format derived from the sequence configuration `yuv420p16le`,
   * the calculated path for the uncompressed depth map sequene of that view is `/Content/A/v3_depth_4096x2048_yuv420p16le.yuv`.
1. Choose an output directory, arbitrarily called `/Experiment` in this description. The directory will be created when it does not yet exist.
1. Finally, assuming that you have built and installed the encoder application, you can start it from the command line:

```shell
/Workspace/tmiv_install/bin/Encoder -n 97 -s A \
    -c /Workspace/tmiv/config/ctc/miv_anchor/A_1_TMIV_encode.json \
    -p configDirectory /Workspace/tmiv/config \
    -p inputDirectory /Content \
    -p outputDirectory /Experiment
```

Whereby `\` is used to indicate line breaks in this manual.

When the same parameter is provided multiple times on the command-line, through `-c` or `-p`, then the right-most argument has precedence.

This will in general result in the following files under the `outputDirectory`:

* A bitstream with the path based on `outputBitstreamPathFmt`, containing metadata and patch data.
* YUV files for each component of each atlas with paths based on `outputGeometryVideoDataPathFmt`, `outputOccupancyVideoDataPathFmt`, `outputTransparencyVideoDataPathFmt` and/or `outputTextureVideoDataPathFmt`. Components may be any of texture, transparency, geometry or occupancy corresponding to the `haveTextureVideo`, `haveTransparencyVideo` :construction:, `haveGeometryVideo` and `haveOccupancyVideo` TMIV encoder parameters.

In this example the following files will be produced:

```
/Experiment/A97/A/TMIV_A97_A.bit
/Experiment/A97/A/TMIV_A97_A_geo_c00_2048x1088_yuv420p10le.yuv
/Experiment/A97/A/TMIV_A97_A_geo_c01_2048x1088_yuv420p10le.yuv
/Experiment/A97/A/TMIV_A97_A_tex_c00_4096x2176_yuv420p10le.yuv
/Experiment/A97/A/TMIV_A97_A_tex_c01_4096x2176_yuv420p10le.yuv
```

### Running the VVenC encoder

As TMIV is agnostic to the 2D video codec, you can use a codec of your choice in out-of-band video coding.
The CTC provide configurations VVenC and VVdeC, so we describe their usage here.
You can alternatively use HEVC (HM) for encoding and decoding.

After TMIV encoding, run VVenC on **all** resulting YUV files.
Per default, executable `vvencFFapp` is available in the TMIV installation directory.
To encode one YUV sequence, use the following settings to run VVenC:

1. Use the supplied configuration file with flag `-c` (there will be a configuration for each component, in this case geometry and texture)
1. Specify the number of input frames (`-f` parameter)
1. Derive the frame width and height from the paths of the YUV files (or encoder log), pass them as `-s WIDTHxHEIGHT`
1. Derive the frame rate (`-fr` parameter) from the sequence configuration (often 30 or 25)
1. Choose a QP (`-q` parameter) or look up in the CTC document. Note that the QPs in the CTC may vary per video component.

For example:

```shell
/Workspace/tmiv_install/bin/vvencFFapp \
  -c /Workspace/tmiv/config/ctc/miv_anchor/A_2_VVenC_encode_tex.cfg \
  -i /Experiment/A97/A/TMIV_A97_A_tex_c00_4096x2176_yuv420p10le.yuv \
  -b /Experiment/A97/A/QP3/TMIV_A97_A_QP3_tex_c00.bit \
  -s 2320x960 -q 30 -f 97 -fr 30
```

In this example the following files will be produced after four invocations:

```
/Experiment/A97/A/QP3/TMIV_A97_A_QP3_geo_c00.bit
/Experiment/A97/A/QP3/TMIV_A97_A_QP3_geo_c01.bit
/Experiment/A97/A/QP3/TMIV_A97_A_QP3_tex_c00.bit
/Experiment/A97/A/QP3/TMIV_A97_A_QP3_tex_c01.bit
```

### Running the VVdeC decoder

After VVenC encoding, run VVdeC on **all** resulting YUV files.
Per default, executable `vvdecapp` is available in the TMIV installation directory.
To decode one YUV sequence, use the following settings to run VVdeC:

1. Specify the input VVC bitstream with parameter `-b`
1. Specify the output yuv file with parameter `-o`

For example:

```shell
/Workspace/tmiv_install/bin/vvdecapp \
  -b /Experiment/A97/A/QP3/TMIV_A97_A_QP3_tex_c00.bit \
  -o /Experiment/A97/A/QP3/TMIV_A97_A_QP3_tex_c00_4096x2176_yuv420p10le.yuv
```

In this example the following files will be produced after four invocations:

```
/Experiment/A97/A/QP3/TMIV_A97_A_QP3_geo_c00_4096x2176_yuv420p10le.yuv
/Experiment/A97/A/QP3/TMIV_A97_A_QP3_geo_c01_4096x2176_yuv420p10le.yuv
/Experiment/A97/A/QP3/TMIV_A97_A_QP3_tex_c00_4096x2176_yuv420p10le.yuv
/Experiment/A97/A/QP3/TMIV_A97_A_QP3_tex_c01_4096x2176_yuv420p10le.yuv
```

### TMIV decoding (with out-of-band video)

For out-of-band decoding, the TMIV decoder requires the TMIV encoded bitstream and the decoded YUV files.
Invoke the decoder with the following arguments:

1. Choose as input directory the output directory of the TMIV encoding step, that is `/Experiment` in this description.
1. Choose as `inputBitstreamPathFmt` a path representing your TMIV bitstream
1. Choose as `inputGeometryVideoFramePathFmt` a path representing the geometry bitstreams
1. Choose as `inputTextureVideoFramePathFmt` a path representing the texture bitstreams
1. Choose an output directory, in this example again `/Experiment` is used to have the viewport videos next to the VVC bitstreams.
1. Define render targets, if any. (The decoder can also produce other outputs such as multiview reconstruction or block to patch maps.)
    * Do not define any render targets for the MIV DSDE anchor.
    * The view name (`-v` argument) may be used multiple times to reconstruct source views and interpolate intermediate views.
    * The pose trace name (`-P` argument) may be used multiple times to render pose trace videos.
    * Rendering tasks are run _sequentially_.
      It is advised to run multiple processes in parallel to speed up anchor generation.
1. Specify the number of output frames (`-N` argument) e.g. 300:
    * For views the actual number of output frames is never more than the number of input frames,
    * For pose traces the sequence of decoded input frames is looped (by mirroring; repeatedly traversing the sequence forth and back) and the number of output frames can exceed the number of input frames.
1. Finally, assuming that you have built and installed the decoder application, you can start it from the command line:

```shell
/Workspace/tmiv_install/bin/Decoder -n 97 -N 300 -s A -r QP3 -v v11 -P p02 \
    -c /Workspace/tmiv/config/ctc/miv_anchor/A_4_TMIV_decode.json \
    -p configDirectory /Workspace/tmiv/config \
    -p inputDirectory /Experiment \
    -p outputDirectory /Experiment
```

When the same parameter is provided multiple times on the command-line, through `-c` or `-p`, then the right-most argument has precedence.

This will in general result in the following files under the `outputDirectory`:

* Block to patch maps with the path based on `outputBlockToPatchMapPathFmt`.
* Reconstructed multiview video based on `outputMultiviewGeometryPathFmt`, `outputOccupancyVideoDataPathFmt`, `outputMultiviewTransparencyPathFmt` and/or `outputMultiviewTexturePathFmt`.
* Rendered viewport videos based on `outputViewportGeometryPathFmt` and/or `outputViewportTexturePathFmt`.
* Reconstructed sequence configuration for each frame at which it changes based on `outputSequenceConfigPathFmt`.

In this example the following files will be produced:

```
/Experiment/A97/A/QP3/A97_A_QP3_p02_tex_2048x2048_yuv420p10le.yuv
/Experiment/A97/A/QP3/A97_A_QP3_v11_tex_4096x2048_yuv420p10le.yuv
```

### Running the TMIV renderer

The TMIV renderer was added to support the MIV decoder-side depth estimating anchor. The application has similar input to the TMIV encoder (input views and camera parameters) and similar output to the TMIV decoder (rendered viewport).

For this example, we will be using the best reference [R_1_TMIV_render.json](/config/ctc/best_reference/R_1_TMIV_render.json) configuration and [A.json](/config/ctc/sequences/A.json) sequence configuration.

1. Place the color and depth videos [[5]](#references) in a folder arbitrarily named `/Content` in this description.
1. Choose an output directory, arbitrarily called `/Experiment` in this description. The directory will be created when it does not yet exist.
1. Define render targets (having none is allowed but not that useful):
   * The view name (`-v` argument) may be used multiple times to reconstruct source views and interpolate intermediate views.
   * The pose trace name (`-P` argument) may be used multiple times to render pose trace videos.
   * Rendering tasks are run _sequentially_. It is adviced to run multiple processes in parallel to speed up anchor generation.
1. Specify the number of input frames (`-n` argument), e.g. 97.
1. Specify the number of output frames (`-N` argument), e.g. 300:
   * For views the actual number of output frames is never more than the number of input frames,
   * For pose traces the input frames are mirrored and the number of output frames can exceed the number of input frames.
1. Finally, assuming that you have built and installed the renderer application, you can start it from the command line:

```shell
/Workspace/tmiv_install/bin/Renderer -n 97 -N 300 -s A -r R0 -v v11 -P p02 \
    -c /Workspace/tmiv/config/ctc/best_reference/R_1_TMIV_render.json \
    -p configDirectory /Workspace/tmiv/config \
    -p inputDirectory /Content \
    -p outputDirectory /Experiment
```

When the same parameter is provided multiple times on the command-line, through `-c` or `-p`, then the right-most argument has precedence.

This will in general result in the following files under the `outputDirectory`:

* Rendered viewport videos based on `outputViewportGeometryPathFmt` and/or `outputViewportTexturePathFmt`.

In this example the following files will be produced:

```
/Experiment/R97/A/R0/R97_A_R0_p02_tex_2048x2048_yuv420p10le.yuv
/Experiment/R97/A/R0/R97_A_R0_v11_tex_4096x2048_yuv420p10le.yuv
```

Note that when running the TMIV renderer to generate the MIV decoder-side depth estimating anchor, the command line to use for reconstructing a source view should be:

```shell
/Workspace/tmiv_install/bin/Renderer -n 17 -N 17 -s A -r QP1 -v v11 \
    -c /Workspace/tmiv/config/ctc/miv_dsde_anchor/G_6_TMIV_render.json \
    -p configDirectory /Workspace/tmiv/config \
    -p inputDirectory /Experiment \
    -p outputDirectory /Experiment
```

This will in general result in the following files under the `outputDirectory`:

```
/Experiment/G17/A/QP1/G17_A_QP1_v11_tex_4096x2048_yuv420p10le.yuv
```

## Instructions for in-band HEVC video coding with HM

The test model has the capability of muxing, demuxing and in-memory decoding of HEVC video using the HEVC Test Model (HM).

### Running the HM encoder

After TMIV encoding (analogous as TMIV encoding for VVC in the CTC), only with the config in `tmiv/config/test/miv_multiplex/V_1_TMIV_encode.json`, run HM on **all** resulting YUV files.
If you have configured the project with `BUILD_HM=ON, BUILD_TAppEncoder=TRUE, and BUILD_TAppDecoder=TRUE`, then the HM executables are available in the TMIV installation directory.
To encode one YUV sequence, use the following settings to run HM:

1. Use the supplied configuration file (there will be a configuration for each component, in this case geometry and texture)
1. Specify the number of input frames (`-f` parameter)
1. Derive the frame width and height from the paths of the YUV files (or encoder log)
1. Derive the frame rate (`-fr` parameter) from the sequence configuration (often 30 or 25)
1. Choose a QP (`-q` parameter) or look up in the CTC document. Note that the QPs in the CTC may vary per video component.

For example:

```shell
/Workspace/tmiv_install/bin/TAppEncoder \
  -c /Workspace/tmiv/config/test/miv_multiplex/V_2_HM_encode_tex.cfg \
  -i /Experiment/V97/A/TMIV_V97_A_tex_c00_4096x2176_yuv420p10le.yuv \
  -b /Experiment/V97/A/QP3/TMIV_V97_A_QP3_tex_c00.bit \
  -wdt 2320 -hgt 960 -q 30 -f 97 -fr 30
```

In this example the following files will be produced after four invocations:

```
/Experiment/V97/A/QP3/TMIV_V97_A_QP3_geo_c00.bit
/Experiment/V97/A/QP3/TMIV_V97_A_QP3_geo_c01.bit
/Experiment/V97/A/QP3/TMIV_V97_A_QP3_tex_c00.bit
/Experiment/V97/A/QP3/TMIV_V97_A_QP3_tex_c01.bit
```

### Running the TMIV multiplexer

1. Choose as input directory the output directory of the HM step, that is `/Experiment` in this description.
1. Choose an output directory, in this example `/Experiment` is used to have the multiplexed bitstream next to the HM bitstreams.
1. The test ID (`-r` argument) is used to tag multiple video encodings at different settings, e.g. QP1, QP2, etc. or R0 for lossless.
1. Finally, assuming that you have built and installed the multiplexer application, you can start it from the command line:

```shell
/Workspace/tmiv_install/bin/Multiplexer -n 97 -s A -r QP3 \
    -c /Workspace/tmiv/config/test/miv_in-band_hm/H_3_TMIV_mux.json \
    -p configDirectory /Workspace/tmiv/config \
    -p inputDirectory /Experiment \
    -p outputDirectory /Experiment
```

When the same parameter is provided multiple times on the command-line, through `-c` or `-p`, then the right-most argument has precedence.

This will in general result in the following file under the `outputDirectory`:

* A bitstream with the path based on `outputBitstreamPathFmt`, containing metadata, patch data and video sub bitstreams.

In this example the following file will be produced:

```
/Experiment/V97/A/QP3/TMIV_V97_A_QP3.bit
```

### Running the TMIV decoder

1. Choose as input directory the output directory of the multiplexing step, that is `/Experiment` in this description.
1. Choose an output directory, in this example again `/Experiment` is used to have the viewport videos next to the HM bitstreams.
1. Define render targets if any. (The decoder can also produce other outputs such as multiview reconstruction or block to patch maps.)
   * The view name (`-v` argument) may be used multiple times to reconstruct source views and interpolate intermediate views.
   * The pose trace name (`-P` argument) may be used multiple times to render pose trace videos.
   * Rendering tasks are run _sequentially_. It is advised to run multiple processes in parallel to speed up anchor generation.
1. Specify the number of output frames (`-N` argument) e.g. 300:
   * For views the actual number of output frames is never more than the number of input frames,
   * For pose traces the sequence of decoded input frames is looped (by mirroring; repeatedly traversing the sequence forth and back) and the number of output frames can exceed the number of input frames.
1. Finally, assuming that you have built and installed the decoder application, you can start it from the command line:

```shell
/Workspace/tmiv_install/bin/Decoder -n 97 -N 300 -s A -r QP3 -v v11 -P p02 \
    -c /Workspace/tmiv/config/test/miv_in-band_hm/H_4_TMIV_decode.json \
    -p configDirectory /Workspace/tmiv/config \
    -p inputDirectory /Experiment \
    -p outputDirectory /Experiment
```

When the same parameter is provided multiple times on the command-line, through `-c` or `-p`, then the right-most argument has precedence.

This will in general result in the following files under the `outputDirectory`:

* Block to patch maps with the path based on `outputBlockToPatchMapPathFmt`.
* Reconstructed multiview video based on `outputMultiviewGeometryPathFmt`, `outputOccupancyVideoDataPathFmt`, `outputMultiviewTransparencyPathFmt` and/or `outputMultiviewTexturePathFmt`.
* Rendered viewport videos based on `outputViewportGeometryPathFmt` and/or `outputViewportTexturePathFmt`.
* Reconstructed sequence configuration for each frame at which it changes based on `outputSequenceConfigPathFmt`.

In this example the following files will be produced:

```
/Experiment/V97/A/QP3/V97_A_QP3_p02_tex_2048x2048_yuv420p10le.yuv
/Experiment/V97/A/QP3/V97_A_QP3_v11_tex_4096x2048_yuv420p10le.yuv
```

## Instructions for multiplane image coding

The multi-plane image (MPI) encoder is able to encode a MPI input consisting of texture and transparency components. No geometry is needed for this test.
The input is given as a succession of very sparse layers both for texture and transparency. In the example given in [M.json](/config/ctc/sequences/M.json), there are 423 such layers.

A specific MPI encoder is to be used to deal with this type of content, because there is no geometry. Please refer to configuration file [M_1_TMIV_encode.json](/config/test/miv_mpi/M_1_TMIV_encode.json) for encoding.
And a specific MPI synthesizer is needed at rendering stage. Please refer to configuration file [M_4_TMIV_decode.json](/config/test/miv_mpi/M_4_TMIV_decode.json) for decoding.

But actually, even if the internal processing is different, the exact same steps as for MIV anchor can be used to encode a bitsream for MPI content and render a viewport:

* MPI test:
  1. Run the TMIV encoder with MPI encoder (see [Running the TMIV encoder](#running-the-tmiv-encoder))
  1. Run the HM encoder on all video sub bitstreams (see [Running the HM encoder](#running-the-hm-encoder))
  1. Run the TMIV multiplexer to form the output bitstream (see [Running the TMIV multiplexer](#running-the-tmiv-multiplexer))
  1. Run the TMIV decoder with MPI synthesizer to decode the bitstream and render a viewport (see [Running the TMIV decoder](#running-the-tmiv-decoder))

The only difference is the absence of geometry (there is no use of `inputGeometryPathFmt`) and the presence of transparency (we do use `inputTransparencyPathFmt`).
And also some specific parameters for [MPI encoder](#mpi-encoder) and [MPI synthesizer](#mpi-synthesizer).

Input texture and transparency files for the example [M.json](/config/ctc/sequences/M.json) are:
```
/Content/SM/mpi_texture_4176x2024_yuv420p10le.yuv
/Content/SM/mpi_transparency_4176x2024_yuv420p.yuv
```

Please refer to the configuration files in [config/test/miv_mpi](/config/test/miv_mpi) to generate the following files on example [M.json](/config/ctc/sequences/M.json).

Following generated atlas for the example will be produced:
```
/Experiment/A17/SM/TMIV_A17_SM.bit
/Experiment/A17/SM/TMIV_A17_SM_tra_c00_4096x4096_yuv420p10le.yuv
/Experiment/A17/SM/TMIV_A17_SM_tex_c00_4096x4096_yuv420p10le.yuv
```

Following HM encoded files at QP3 for the example will be produced:
```
/Experiment/A1/SM/QP3/TMIV_A17_SM_QP3_tra_c00.bit
/Experiment/A1/SM/QP3/TMIV_A17_SM_QP3_tex_c00.bit
```

Following multiplexed file for the example will be produced:
```
/Experiment/A1/SM/QP3/TMIV_A17_SM_QP3.bit
```

And a synthesized pose trace for the example will look like:
```
/Experiment/A1/SM/QP3/A17_SM_QP3_p01_tex_1920x1080_yuv420p10le.yuv
```

# Overview of TMIV configuration files

## Basic structure of the TMIV configuration files

The TMIV configuration files use the [JSON](http://json.org) file format. The main parameters are key-value pairs of the root object, for example:

```json
{
  "intraPeriod": 32,
  "blockSizeDepthQualityDependent": [ 16, 32 ]
}
```

Selectable components follow this pattern:

```json
{
  "PrunerMethod": "HierarchicalPruner",
  "HierarchicalPruner": {
    "erode": 3,
    "etcetera": "..."
  }
}
```

In some cases there is a bypass component, e.g. `NoPruner` or `NoCuller`. Even though these have no parameters, the key still needs to be provided:

```json
{
  "PrunerMethod": "NoPruner",
  "NoPruner": { }
}
```

## Input/output parameters

TMIV constructs input and output paths using:
1. base directories `configDirectory`, `inputDirectory` and `outputDirectory`,
1. path formats with [{fmt}](https://github.com/fmtlib/fmt) syntax.

The [format syntax](https://fmt.dev/latest/syntax.html#syntax) is convenient and flexible. It is not required to use all available placeholders and with positional placeholders it is possible to use the same placeholder multiple times.

### Base directories

* **configDirectory**: the base directory for the configuration files.
* **inputDirectory**: the base directory for the input files.
* **outputDirectory**: the base directory for the output files.

An experiment may have multiple steps, e.g. Encoder, Multiplexer, Decoder, in which case the output directory of one step may correspond to the input directory of the next step.

Base directories can be overriden by using absolute paths for the path formats:
* `/my/output/dir` plus `my_filename.yuv` gives `/my/output/dir/my_filename.yuv`
* `/my/output/dir` plus `/my/path.yuv` gives `/my/path.yuv`

### Path format placeholders

Depending on the situation, the following placeholders are available to form paths:

* **Atlas ID** (e.g. 12) as read from the bitstream
* **Atlas index** in `0 .. atlas count - 1`, as determined by the test model
* **Content ID** (e.g. J for Kitchen), as specified by the `-s` command-line parameter
* **Frame height** (e.g. 1080) as determined by the test model
* **Frame index** is 0 for the Encoder and in the range `0 .. number of input frames - 1` for the Renderer
* **Frame width** (e.g. 1920) as determined by the test model
* **Number of input frames** (e.g. 97), as specified by the `-n` command-line parameter
* **Number of output frames** (e.g. 300), as specified by the `-N` command-line parameter
* **Pose trace name** (e.g. p02) as specified using the `-P` command-line parameter
* **Test ID** (e.g. R0 or QP3) as specified by the `-r` command-line parameter
* **Video format** (e.g. yuv420p10le) as determined by the test model
* **View index** in `0 .. view count - 1`, as determined by the test model
* **View name** (e.g. v11) as specified using the `-v` command-line parameter or determined by the test model

### Input path formats

Unless specified otherwise, the base directory for these path formats is `inputDirectory`.

* **inputBitstreamPathFmt**: the path format of the input bitstream, consumed by the Multiplexer and Decoder, with placeholders:
  * 0: number of input frames,
  * 1: content ID,
  * 2: test ID.
* **inputGeometryPathFmt**: the path format of the multiview uncompressed geometry (depth) data, consumed by the Encoder and Renderer, with placeholders:
  * 0: number of input frames,
  * 1: content ID,
  * 2: test ID,
  * 3: view name,
  * 4, 5, 6: frame width, frame height, and video format.
* **inputTexturePathFmt**: the path format of the multiview uncompressed texture (color) data, consumed by the Encoder and Renderer, with the same placeholders as `inputGeometryPathFormat`.
* **inputTransparencyPathFmt**: the path format of the multiview uncompressed transparency (alpha) data, with the same placeholders as `inputTransparencyPathFormat`.
* **inputEntityPathFmt**: the path format of the multiview uncompressed entity maps, consumed by the Encoder, with the same placeholders as `inputGeometryPathFormat`.
* **inputGeometryVideoFramePathFmt**: the path format of the uncompresed geometry video data (GVD), consumed by the Decoder for out-of-band video decoding, e.g. for testing alternative video codecs, with placeholders:
  * 0: number of input frames,
  * 1: content ID,
  * 2: test ID,
  * 3: atlas ID,
  * 4, 5: frame width and height.
* **inputOccupancyVideoFramePathFmt**: the path format of the uncompresed occupancy video data (OVD), consumed by the Decoder for out-of-band video decoding, e.g. for testing alternative video codecs, with the same placeholders as `inputGeometryVideoFramePathFmt`.
* **inputTextureVideoFramePathFmt**: the path format of the uncompresed attribute video data (AVD) with attribute ID `ATTR_TEXTURE`, consumed by the Decoder for out-of-band video decoding, e.g. for testing alternative video codecs, with the same placeholders as `inputGeometryVideoFramePathFmt`.
* **inputTransparencyVideoFramePathFmt**: the path format of the uncompresed attribute video data (AVD) with attribute ID `ATTR_TRANSPARENCY`, with the same placeholders as `inputGeometryVideoFramePathFmt`.
* **inputGeometryVsbPathFmt**: the path format of the geometry video sub-bitstream, consumed by the Multiplexer, with placeholders:
  * 0: number of input frames,
  * 1: content ID,
  * 2: test ID,
  * 3: atlas index.
* **inputOccupancyVsbPathFmt**: the path format of the occupancy video sub-bitstream, consumed by the Multiplexer, with the same placeholders as `inputGeometryVsbPathFmt`.
* **inputTextureVsbPathFmt**: the path format of the attribute video sub-bitstream with attribute ID `ATTR_TEXTURE`, consumed by the Multiplexer, with placeholders:
  * 0: number of input frames,
  * 1: content ID,
  * 2: test ID,
  * 3: atlas index,
  * 4: attribute index.
* **inputNormalVsbPathFmt**: the path format of the attribute video sub-bitstream with attribute ID `ATTR_NORMAL`, consumed by the Multiplexer, with the same placeholders as `inputTextureVsbPathFmt`.
* **inputTransparencyVsbPathFmt**: the path format of the attribute video sub-bitstream with attribute ID `ATTR_TRANSPARENCY`, consumed by the Multiplexer, with the same placeholders as `inputTextureVsbPathFmt`.
* **inputMaterialIdVsbPathFmt**: the path format of the attribute video sub-bitstream with attribute ID `ATTR_MATERIAL_ID`, consumed by the Multiplexer, with the same placeholders as `inputTextureVsbPathFmt`.
* **inputReflectanceVsbPathFmt**: the path format of the attribute video sub-bitstream with attribute ID `ATTR_REFLECTANCE`, consumed by the Multiplexer, with the same placeholders as `inputTextureVsbPathFmt`.
* **inputPoseTracePathFmt**: the path format of the pose trace CSV file, with `configDirectory` as base directory and placeholders:
  * 0: number of input frames,
  * 1: content ID,
  * 2: test ID,
  * 3: pose trace name.
* **inputSequenceConfigPathFmt**: the path format of the sequence (content) configuration file, consumed by the Encoder and Renderer to load (reconstructed) source parameters, with `inputDirectory` as primary (try first) and `configDirectory` as secondary (try next) base directory, with placeholders:
  * 0: number of input frames,
  * 1: content ID,
  * 2: test ID,
  * 3: frame index.
* **inputViewportParamsPathFmt**: the path format of the sequence (content) configuration file, to load viewport parameters, consumed by the Decoder and Renderer, with `configDirectory` as base directory and with placeholders:
  * 0: number of input frames,
  * 1: content ID,
  * 2: test ID.

### Output path formats

Unless specified otherwise, the base directory for these path formats is `outputDirectory`.

* **outputBitstreamPathFmt**: the path format of the output bitstream, produced by the Encoder and Multiplexer, with the same placeholders as `inputBitstreamPathFmt`.
* **outputBlockToPatchMapPathFmt**: the path format of the block-to-patch map output, produced by the Decoder, with placeholders:
  * 0: number of input frames,
  * 1: content ID,
  * 2: test ID,
  * 3: atlas index,
  * 4, 5: frame width and height.
* **outputGeometryVideoDataPathFmt**: the path format of the uncompressed geometry video data (GVD), produced by the Encoder, with the same placeholders as `outputBlockToPatchMapPathFmt`.
* **outputOccupancyVideoDataPathFmt**: the path format of the uncompressed occupancy video data (GVD), produced by the Encoder, with the same placeholders as `outputBlockToPatchMapPathFmt`.
* **outputTextureVideoDataPathFmt**: the path format of the uncompressed attribute video data (GVD) with attribute ID `ATTR_TEXTURE`, produced by the Encoder, with the same placeholders as `outputBlockToPatchMapPathFmt`.
* **outputTransparencyVideoDataPathFmt**: the path format of the uncompressed attribute video data (GVD) with attribute ID `ATTR_TRANSPARENCY`, with the same placeholders as `outputBlockToPatchMapPathFmt`.
* **outputMultiviewGeometryPathFmt**: the path format of the reconstructed multiview geometry (depth) data, produced by the Decoder, with placeholders:
  * 0: number of input frames,
  * 1: content ID,
  * 2: test ID,
  * 3: view index,
  * 4, 5: frame width and height.
* **outputMultiviewOccupancyPathFmt**: the path format of the reconstructed (pruned) multiview occupancy data, produced by the Decoder, with the same placeholders as `outputMultiviewGeometryPathFmt`.
* **outputMultiviewTexturePathFmt**: the path format of the reconstructed (pruned) multiview texture (color) data, produced by the Decoder, with the same placeholders as `outputMultiviewGeometryPathFmt`.
* **outputMultiviewTransparencyPathFmt**: the path format of the reconstructed (pruned) multiview transparency (alpha) data, produced by the Decoder, with the same placeholders as `outputMultiviewGeometryPathFmt`.
* **outputSequenceConfigPathFmt**: the path format of the reconstructed sequence (content) configuration file, produced by the Decoder, written for each frame whereby parameters have changed, with the same placeholders as `inputSequenceConfigPathFmt`.
* **outputViewportGeometryPathFmt**: the path format of the geometry (depth) video data of the rendered viewport, produced by the Decoder or Renderer, with placeholders:
  * 0: number of input frames,
  * 1: content ID,
  * 2: test ID,
  * 3: number of output frames,
  * 4: view or pose trace name,
  * 5, 6, 7: frame width and height, and video format.
* **outputViewportTexturePathFmt**: the path format of the texture (color) video data of the rendered viewport, produced by the Decoder or Renderer, with the same placeholders as `outputViewportGeometryPathFmt`.

## Algorithmic parameters

Some of the algorithmic components of the test model have parameters. This section provides a short description of these parameters in reference to the _test model_ [[4]](#references).

Some general parameters are defined in the root of the configuration file. All others are defined in the node of a component.

### General parameters

These parameters are in the root of the configuration file and may be accessed by multiple components:

* Output video sub-bitstreams:
  * **haveOccupancyVideo:** bool; output occupancy video data (OVD) instead of  depth/occupancy coding within geometry video data (GVD). Make sure to use ExplicitOccupancy as the geometry quantizer.
  * **haveTextureVideo:** bool; output attribute video data (AVD) to encode the texture attribute. When false texture data is still needed as input of the test model.
  * **haveTransparencyVideo:** bool; output attribute video data (AVD) to encode the transparency attribute. :construction: 
  * **haveGeometryVideo:** bool; output geometry video data (GVD) to encode depth and optionally also occuapncy information. Without geometry, depth estimation is shifted from a pre-encoding to a post-decoding process.
  * **geometryScaleEnabledFlag:** bool; when true geometry is downscaled by a factor of two in respect to the atlas frame size. Otherwise geometry is at full resolution.
* Atlas frame size calculation and packing:
  * **intraPeriod:** int; the intra patch frame period. This is the step in frame order count between consecutive frames that have an atlas tile layer of type I_TILE. The test model is not aware of the intra period of the video codec. This other intra period is configured independently.
  * **maxAtlases:** int; the maximum number of transmitted atlases in total.
  * **maxEntityId:** int; the maximum ID of entities, whereby the default "0" disables entity-based coding.
  * **maxLumaSamplerate:** float; the maximum number of luma samples per second counted over all atlases, groups and components. This parameter communicates the equally-named CTC constraint.
  * **maxLumaPictureSize:** int; the maximum number of samples per atlas frame, which corresponds to the maximum number of samples per texture attribute video frame. This parameter communicates the equally-named CTC constraint.
  * **numGroups:** int; the number of groups of atlases:
     * 0: No grouping, each atlas is independently useful for rendering (as with the MIV view anchor)
     * 1: Single group, all atlases are needed for rendering (as with the MIV anchor)
     * 2: _N_ groups, with the group-based encoder, partitions views to independently encode multiple groups of atlases.
* Metadata:
  * **OmafV1CompatibleFlag:** bool; when enabled the equally-named flag is written in the bitstream.
  * **dqParamsPresentFlag:** bool; optional parameter, when false the depth quantization parameters are not written to the bitstream.
  * **randomAccess:** bool; when true complete MIV metadata (inclding VPS, CAD, AD V3C units) are written per IRAP to the bitstream such that there is no dependency when parsing metadata between different IRAPs.

### Depth quality assessor

* **blendingFactor:** float; for every reprojected pixel it is checked if
reprojected geometry value is higher than 1.0 - _blendingFactor_ of geometry
value of collocated pixel or any of its neighbors in the target view.
* **maxOutlierRatio:** float; pixel outlier threshold above which the geometry
quality is judged to be low.

### Encoder and group-based encoder

Most of the parameters are defined in the root. The exception is:

* **dilate:** int; number of dilation steps on the aggregated pruning mask.
 This parameter is only in effect for low depth quality.

### Geometry quantizer

* **depthOccThresholdIfSet:** int; the value of the depth-occupancy map
 threshold when occupancy information is encoded in the geometry video data of
 a specific view.

### Hierarchical pruner

* **depthParameter:** float; weighting parameter of depth ordering in the trial
 view synthesis that is performed as part of the pruning process.
* **rayAngleParameter:** float; weighting parameter of ray angle in the trial
 view synthesis that is perforemd as part of the pruning process.
* **stretchingParameter:** float; weighting parameter of triangle stretching in
 in the trial view synthesis that is performed as part of the pruning process.
* **maxStretching:** float; the maximum stretching of a trangle in pixel units
 above which the triangle is dropped.
* **dilate:** int; number of dilation steps on the pruning mask.
* **erode:** int; number of erosion steps on the pruning mask.
* **maxDepthError:** float; the maximum relative difference in depth value below which
 the pixel is pruned.
* **maxLumaError:** float; the maximum difference in texture value [0, 1]
 below which the pixel is pruned.
* **maxBasicViewsPerGraph:** int; parameter to control the maximum number of basic
 views per pruning cluster.

### Packer

* **MinPatchSize:** int; is the number of pixels of the smallest border of the
 patch, below which the patch is discarded. Default value is 8.
* **Overlap:** int; is the number of pixels which will be added to a frontier
 of a newly split patch; it prevents seam artefacts. Default value is 1.
* **PiP:** int; is a flag enabling the Patch-in-Patch feature when equal to 1.
 It allows the insertion of patches into other patches. Default value is 1.
* **enableMerging:** bool; enable the patch merging step.
* **sortingMethod:** int; code for the sorting method of clusters during packing
 step in [0 (by descending area), 1 (by increasing view index)].
* **enableRecursiveSplit:** bool; enable the recursive split of clusters.

### Basic view allocator

* **maxBasicViewFraction:** float; the maximum number of available luma samples
 that is used for coding views completely.
* **outputAdditionalViews:** bool; when true output basic views and additional
 views. when false output only basic views.
* **minNonCodedViews:** int; the minimum number of source views that will not
 be coded as basic view.

### Server-side Inpainter

* **SubMethod:** string; underlying ViewOptimizerMethod.
* **resolution:** int[2]; resolution [hor,vert] of the inpainted view.
* **SynthesizerMethod:** string; the sythesizer used for creating an inpainted background view.
* **InpainterMethod:** string; the actual inpainting method used to inpaint the hidden regions. 
* **blurRadius:** int; radius of the blur kernel used to locate regions of missing data.
* **inpaintThreshold:** int; a comparison threshold used to locate regions of missing data.
* **fieldOfViewMargin:** float; the field-of-view of the inpainted view is the accumulative field-of-view of the source-views but expanded with this factor.

### Decoder

* **geometryEdgeMagnitudeTh:** int; parameter of the geometry upscaling, in
 line with the hypothetical reference renderer.
* **maxCurvature:** int;  parameter of the geometry upscaling, in line with the
 hypothetical reference renderer.
* **minForegroundConfidence:** float; parameter of the geometry upscaling, in
 line with the hypothetical reference renderer.

### AdditiveSynthesizer

* **maxStretching:** float; used to discard too large triangles that yield large gaps in the target view.
* **depthParameter:** float; blending parameter that prioritizes foreground over background. A negative value blends background over foreground.
* **rayAngleParameter:** float; blending parameter that prioritizes on ray-angle.
* **stretchingParameter:** float; blending parameter that prioritizes on triangle-stretching (de-occlusion).

### ViewWeightingSynthesizer

* **angularScaling:** float; Drives the splat size at the warping stage.
* **minimalWeight:** float; Allows for splat degeneracy test at the warping stage.
* **stretchFactor:** float; Limits the splat max size at the warping stage.
* **overloadFactor:** float; Geometry selection parameter at the selection stage.
* **filteringPass:** int; Number of median filtering pass to apply to the visibility map.
* **blendingFactor:** float; Used to control the blending at the shading stage.

### MPI encoder

* **TextureDilation:** int; Number of dilations steps for the the texture atlas.
* **TransparencyDynamic:** int; The number of transparency levels in the transparency atlas is given by pow(2, TransparencyDynamic).

### MPI synthesizer

* **minAlpha:** float; Drives the blending process at shading stage.

# Instruction to use TMIV as a library

In the `CMakeLists.txt` of the larger project, use the line:

```CMake
find_package(TMIV 7.0.0 REQUIRED)
```

The `TMIV_DIR` variable needs to be set to the directory named `/Workspace/tmiv_install/lib/cmake/TMIV` or `/Workspace/tmiv_install/lib64/cmake/TMIV` depending on the platform. 

After that the targets `TMIV::DecoderLib`, `TMIV::EncoderLib`, etc. are available as target dependencies.

# Structure of the test model

This software consists of multiple executables and static libraries. Below figure is the CMake module dependency graph of TMIV 5.0. The most important executables are Encoder and Decoder. When enabled, the project also includes the HM executables TAppEncoder and TAppDecoder.

![CMake module graph](doc/module_graph.svg)

Each module consists of one or more components. Most components derive from an interface and some interfaces have multiple alternative components. For instance, the ViewOptimizerLib includes an IViewOptimizer interface with NoViewOptimizer and ViewReducer components that implement that interface.

# Contributing to the test model

Core experiments are expected to include the reference software as a subproject
and introduce new components. Alternatively core experiments may branch the test
model. Contributions should be in the form of git merge requests to the
MPEG-internal repository. See [doc/CONTRIBUTING.md](doc/CONTRIBUTING.md) for further info.

# References

* [1] *Call for Proposals on 3DoF+ Visual*, ISO/IEC JTC 1/SC 29/WG 11 N 18145, Jan. 2019, Marrakesh, Morocco.
* [2] J. Boyce, R. Doré, V. Kumar Malamal Vadakital (Eds.), *Potential Improvements of MPEG Immersive Video*, [ISO/IEC JTC 1/SC 29/WG 04 N 0004](https://www.mpegstandards.org/wp-content/uploads/mpeg_meetings/132_OnLine/w19677.zip), October 2020, Online.
* [3] J. Boyce, B. Chupeau, L. Kondrad (Eds.), *Text of ISO/IEC DIS 23090-12 MPEG Immersive Video*, ISO/IEC JTC 1/SC 29/WG 04 N 0049, January 2021, Online.
* [4] B. Salahieh, C. Bachhuber, J. Jung, A. Dziembowski (Eds.), *Test Model 8 for MPEG Immersive Video*, [ISO/IEC JTC 1/SC 29/WG 04 N 0050](https://www.mpegstandards.org/wp-content/uploads/mpeg_meetings/133_OnLine/w20002.zip), January 2021, Online.
* [5] J. Jung, B. Kroon (Eds.), *Common Test Conditions for MPEG Immersive Video*, [ISO/IEC JTC 1/SC 29/WG 04 N 0051](https://www.mpegstandards.org/wp-content/uploads/mpeg_meetings/133_OnLine/w20003.zip), January 2021, Online.

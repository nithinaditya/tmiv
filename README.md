## TMIV Build and Installation Instructions

 ISO C++17 conformant and requires the following external libraries:

* The [{fmt}](https://github.com/fmtlib/fmt) library for string formatting
* The [Catch2](https://github.com/catchorg/Catch2) test framework
* The [HEVC test model](https://vcgit.hhi.fraunhofer.de/jvet/HM) (HM)

#### Minimal prerequisites are:

* C++17 compiler and build tools
* CMake 3.14 or newer

#### The first step is to attain all external libraries
* #### Catch2 testing framework

   * Visit this URL: https://github.com/catchorg/Catch2/tree/v2.11.1
   * Download ZIP or clone the repo resulting in a directory /Workspace/Catch2-2.11.1 such that the file /Workspace/Catch2-2.11.1/README.md exists*.


* #### {fmt} string formatting library

   * Visit this URL: https://github.com/fmtlib/fmt/tree/7.0.3
   * Download ZIP or clone the repo resulting in a directory /Workspace/fmt-7.0.3 such that the file /Workspace/fmt-7.0.3/README.md exists.


* #### HEVC test model (HM)

    * Visit this URL: https://vcgit.hhi.fraunhofer.de/jct-vc/HM/-/tree/HM-16.16
    * Download ZIP or clone the repo resulting in a directory /Workspace/HM-HM-16.16 such that the file /Workspace/HM-HM-16.16/README exists.
    * Rename that directory to /Workspace/HM-16.16.


* #### Fraunhofer Versatile Video Encoder (VVenC)

   * Visit this URL: https://github.com/fraunhoferhhi/vvenc/tree/v0.2.0.0
   * Download ZIP 
   * Unzip, resulting in a directory /Workspace/vvenc-0.2.0.0 such that the file /Workspace/vvenc-0.2.0.0/README.md exists.


* #### Fraunhofer Versatile Video Decoder (VVdeC)

   * Visit this URL: https://github.com/fraunhoferhhi/vvdec/tree/v0.2.0.0
   * Donwload ZIP
   * Unzip, resulting in a directory /Workspace/vvdec-0.2.0.0 such that the file /Workspace/vvdec-0.2.0.0/README.md exists.

#### To install the TMIV project
* Visit this URL: https://gitlab.com/mpeg-i-visual/tmiv/-/tree/v8.0.1
* Download ZIP or clone the repo resulting in a directory /Workspace/tmiv-v6.1 such that the file /Workspace/tmiv-v6.1/README.md exists.
* Rename to /Workspace/tmiv to match with the following instructions.

<br>

### Building and Installing TMIV
To build and install TMIV into the directory /Workspace/tmiv_install with sources in the directory /Workspace/tmiv, execute the following on the command line:
```
mkdir /Workspace/tmiv_build
cd /Workspace/tmiv_build
cmake -DCMAKE_INSTALL_PREFIX=/Workspace/tmiv_install -DCMAKE_BUILD_TYPE=Release /Workspace/tmiv
cmake --build . --parallel --config Release
cmake --build . --parallel --config Release --target install
```
<br>

### Installation Results
After installation, the TMIV executables Encoder, Decoder and Renderer will be available under the directory /Workspace/tmiv_install/bin.
By default TMIV only builds the HM modules that are required for TMIV (TLibCommon and TLibDecoder).
When HM_BUILD_TAPPDECODER and HM_BUILD_TAPPENCODER are selected, then the TAppDecoder and TAppEncoder tools respectively will also be installed to this directory.
TMIV per default builds vvencFFapp and vvdecapp from the Fraunhofer VVC implementations.

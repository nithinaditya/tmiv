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
   * Click on the green button "Code" and select "Download ZIP"
   * Unzip, resulting in a directory /Workspace/Catch2-2.11.1 such that the file /Workspace/Catch2-2.11.1/README.md exists*.


* #### {fmt} string formatting library

   * Visit this URL: https://github.com/fmtlib/fmt/tree/7.0.3
   * Click on the green button "Code" and select "Download ZIP"
   * Unzip, resulting in a directory /Workspace/fmt-7.0.3 such that the file /Workspace/fmt-7.0.3/README.md exists.


* #### HEVC test model (HM)

    * Visit this URL: https://vcgit.hhi.fraunhofer.de/jct-vc/HM/-/tree/HM-16.16
    * Click on the download button (next to the Clone button) and select "zip"
    * Unzip, resulting in a directory /Workspace/HM-HM-16.16 such that the file /Workspace/HM-HM-16.16/README exists.
    * Rename that directory to /Workspace/HM-16.16.


* #### Fraunhofer Versatile Video Encoder (VVenC)

   * Visit this URL: https://github.com/fraunhoferhhi/vvenc/tree/v0.2.0.0
   * Click on the green "Code" button and select "Download ZIP"
   * Unzip, resulting in a directory /Workspace/vvenc-0.2.0.0 such that the file /Workspace/vvenc-0.2.0.0/README.md exists.


* #### Fraunhofer Versatile Video Decoder (VVdeC)

   * Visit this URL: https://github.com/fraunhoferhhi/vvdec/tree/v0.2.0.0
   * Click on the green "Code" button and select "Download ZIP"
   * Unzip, resulting in a directory /Workspace/vvdec-0.2.0.0 such that the file /Workspace/vvdec-0.2.0.0/README.md exists.

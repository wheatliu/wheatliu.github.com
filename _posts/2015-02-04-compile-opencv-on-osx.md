---
layout: blog
title: osx 平台编译安装opencv3.0 beta 
---

osx平台编译安装opencv3.0-beta
---------------------------
1. 安装cmake
``` bash
brew install cmake
```
2. 下载opecv3.0 beta source code
``` bash
wget <https://github.com/Itseez/opencv/archive/3.0.0-beta.zip>
unzip 3.0.0-beta.zip
```
3. compile && install
``` bash
cd opencv-3.0.0-beta
mkdir build && cd build
# warning： BUILD_NEW_PYTHON_SUPPORT=ON （python2.7）
cmake -D WITH_TBB=ON -D BUILD_NEW_PYTHON_SUPPORT=ON -D WITH_V4L=ON -D INSTALL_C_EXAMPLES=ON -D INSTALL_PYTHON_EXAMPLES=ON -D BUILD_EXAMPLES=ON -D WITH_IPP=OFF -D CMAKE_INSTALL_PREFIX=/usr/local ..
# 以下为程序输出
--
-- General configuration for OpenCV 3.0.0-beta =====================================
--   Version control:               unknown
--
--   Platform:
--     Host:                        Darwin 14.1.0 x86_64
--     CMake:                       3.1.1
--     CMake generator:             Unix Makefiles
--     CMake build tool:            /usr/bin/make
--     Configuration:               Release
--
--   C/C++:
--     Built as dynamic libs?:      YES
--     C++ Compiler:                /usr/bin/c++  (ver 6.0.0.6000056)
--     C++ flags (Release):         -fsigned-char -W -Werror=return-type -Werror=non-virtual-dtor -Werror=address -Werror=sequence-point -Wformat -Werror=format-security -Wmissing-declarations -Wmissing-prototypes -Wstrict-prototypes -Wundef -Winit-self -Wpointer-arith -Wshadow -Wsign-promo -Wno-narrowing -Wno-delete-non-virtual-dtor -Wno-unnamed-type-template-args -fdiagnostics-show-option -Wno-long-long -Wno-semicolon-before-method-body -fno-omit-frame-pointer -msse -msse2 -msse3 -fvisibility=hidden -fvisibility-inlines-hidden -O3 -DNDEBUG  -DNDEBUG
--     C++ flags (Debug):           -fsigned-char -W -Werror=return-type -Werror=non-virtual-dtor -Werror=address -Werror=sequence-point -Wformat -Werror=format-security -Wmissing-declarations -Wmissing-prototypes -Wstrict-prototypes -Wundef -Winit-self -Wpointer-arith -Wshadow -Wsign-promo -Wno-narrowing -Wno-delete-non-virtual-dtor -Wno-unnamed-type-template-args -fdiagnostics-show-option -Wno-long-long -Wno-semicolon-before-method-body -fno-omit-frame-pointer -msse -msse2 -msse3 -fvisibility=hidden -fvisibility-inlines-hidden -g  -O0 -DDEBUG -D_DEBUG
--     C Compiler:                  /usr/bin/cc
--     C flags (Release):           -fsigned-char -W -Werror=return-type -Werror=non-virtual-dtor -Werror=address -Werror=sequence-point -Wformat -Werror=format-security -Wmissing-declarations -Wmissing-prototypes -Wstrict-prototypes -Wundef -Winit-self -Wpointer-arith -Wshadow -Wsign-promo -Wno-narrowing -Wno-delete-non-virtual-dtor -Wno-unnamed-type-template-args -fdiagnostics-show-option -Wno-long-long -Wno-semicolon-before-method-body -fno-omit-frame-pointer -msse -msse2 -msse3 -fvisibility=hidden -fvisibility-inlines-hidden -O3 -DNDEBUG  -DNDEBUG
--     C flags (Debug):             -fsigned-char -W -Werror=return-type -Werror=non-virtual-dtor -Werror=address -Werror=sequence-point -Wformat -Werror=format-security -Wmissing-declarations -Wmissing-prototypes -Wstrict-prototypes -Wundef -Winit-self -Wpointer-arith -Wshadow -Wsign-promo -Wno-narrowing -Wno-delete-non-virtual-dtor -Wno-unnamed-type-template-args -fdiagnostics-show-option -Wno-long-long -Wno-semicolon-before-method-body -fno-omit-frame-pointer -msse -msse2 -msse3 -fvisibility=hidden -fvisibility-inlines-hidden -g  -O0 -DDEBUG -D_DEBUG
--     Linker flags (Release):
--     Linker flags (Debug):
--     Precompiled headers:         NO
--
--   OpenCV modules:
--     To be built:                 core flann imgproc imgcodecs videoio highgui ml features2d calib3d objdetect photo video shape stitching superres videostab python2 ts
--     Disabled:                    world
--     Disabled by dependency:      -
--     Unavailable:                 androidcamera cuda cudaarithm cudabgsegm cudacodec cudafeatures2d cudafilters cudaimgproc cudalegacy cudaoptflow cudastereo cudawarping cudev java python3 viz
--
--   GUI:
--     QT:                          NO
--     Cocoa:                       YES
--     OpenGL support:              NO
--     VTK support:                 NO
--
--   Media I/O:
--     ZLib:                        build (ver 1.2.8)
--     JPEG:                        build (ver 90)
--     WEBP:                        build (ver 0.3.1)
--     PNG:                         build (ver 1.5.12)
--     TIFF:                        build (ver 42 - 4.0.2)
--     JPEG 2000:                   build (ver 1.900.1)
--     OpenEXR:                     build (ver 1.7.1)
--     GDAL:                        NO
--
--   Video I/O:
--     DC1394 1.x:                  NO
--     DC1394 2.x:                  NO
--     FFMPEG:                      YES
--       codec:                     YES (ver Unknown)
--       format:                    YES (ver Unknown)
--       util:                      YES (ver Unknown)
--       swscale:                   YES (ver Unknown)
--       gentoo-style:              YES
--     OpenNI:                      NO
--     OpenNI PrimeSensor Modules:  NO
--     OpenNI2:                     NO
--     PvAPI:                       NO
--     GigEVisionSDK:               NO
--     QuickTime:                   NO
--     QTKit:                       YES
--     V4L/V4L2:                    NO/NO
--     XIMEA:                       NO
--
--   Other third-party libraries:
--     Use IPP:                     NO
--     Use IPP Async:               NO
--     Use Eigen:                   NO
--     Use TBB:                     NO
--     Use OpenMP:                  NO
--     Use GCD                      YES
--     Use Concurrency              NO
--     Use C=:                      NO
--     Use Cuda:                    NO
--     Use OpenCL:                  YES
--
--   OpenCL:
--     Version:                     static
--     libraries:                   -framework OpenCL
--     Use AMDFFT:                  NO
--     Use AMDBLAS:                 NO
--
--   Python 2:
--     Interpreter:                 /usr/bin/python2.7 (ver 2.7.6)
--     Libraries:                   /usr/lib/libpython2.7.dylib (ver 2.7.6)
--     numpy:                       /Library/Python/2.7/site-packages/numpy/core/include (ver 1.9.1)
--     packages path:               lib/python2.7/site-packages
--
--   Python 3:
--     Interpreter:                 /usr/local/bin/python3.4 (ver 3.4.2)
--
--   Python (for build):            /usr/bin/python2.7
--
--   Java:
--     ant:                         NO
--     JNI:                         /System/Library/Frameworks/JavaVM.framework/Headers /System/Library/Frameworks/JavaVM.framework/Headers /System/Library/Frameworks/JavaVM.framework/Headers
--     Java wrappers:               NO
--     Java tests:                  NO
--
--   Matlab:
--     mex:                         NO
--
--   Documentation:
--     Build Documentation:         NO
--     Sphinx:                      NO
--     PdfLaTeX compiler:           NO
--     PlantUML:                    NO
--     Doxygen:                     NO
--
--   Tests and samples:
--     Tests:                       YES
--     Performance tests:           YES
--     C/C++ Examples:              YES
--
--   Install path:                  /usr/local
--
--   cvconfig.h is in:              /Users/wheat/opencv-3.0.0-beta/build
-- -----------------------------------------------------------------
--
-- Configuring done
#
make -j $CPU_COUNTS
make install
```
4 在python中使用openvcv
先将cv2.so copy到python查找路径中
``` bash
cp /usr/local/lib/python2.7/site-packages/cv2.so /Library/Python/2.7/site-packages

➜  build  ipython
Python 2.7.6 (default, Sep  9 2014, 15:04:36)
Type "copyright", "credits" or "license" for more information.

IPython 2.3.0 -- An enhanced Interactive Python.
?         -> Introduction and overview of IPython's features.
%quickref -> Quick reference.
help      -> Python's own help system.
object?   -> Details about 'object', use 'object??' for extra details.

In [1]: import cv2

In [2]:
```

到这就完了

[0]: https://elementztechblog.wordpress.com/2014/09/10/405/ "Installing OpenCV 3.0.0 alpha in Ubuntu"
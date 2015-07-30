---
layout: blog
title: osx 平台编译安装opencv3.0 beta 
---

. 安装cmake

``` bash
brew install cmake
```
. 下载opecv3.0 beta source code

``` bash
wget https://github.com/Itseez/opencv/archive/3.0.0-beta.zip
unzip 3.0.0-beta.zip
```


. compile && install

``` bash
cd opencv-3.0.0-beta
mkdir build && cd build
git clone git@github.com:Itseez/opencv_contrib.git
# warning： BUILD_NEW_PYTHON_SUPPORT=ON （python2.7）
cmake -D OPENCV_EXTRA_MODULES_PATH=./opencv_contrib/modules \
        -D CMAKE_INSTALL_PREFIX=/usr/local \
        -D CMAKE_BUILD_TYPE=RELEASE \
        -D BUILD_PYTHON_SUPPORT=ON \
        -D WITH_OPENGL=ON \
        -D WITH_TBB=ON \
        -D BUILD_EXAMPLES=ON \
        -D BUILD_NEW_PYTHON_SUPPORT=ON \
        -D WITH_V4L=ON \
        -D WITH_QT=ON ..
```


. 在python中使用

``` bash
cp /usr/local/lib/python2.7/site-packages/cv2.so /Library/Python/2.7/site-packages

# ipython
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


到这就完了.

可是又有他妈的什么用呢

大把的时间浪费在安装上了

杂草一年一年在我心里长

Reference
----
1. [Installing OpenCV 3.0.0 alpha in Ubuntu](https://elementztechblog.wordpress.com/2014/09/10/405/)

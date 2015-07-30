---
layout: blog
title: 静态编译ffmpeg
---

#debian下静态编译ffmpeg
_____________________

>* 下载各类编码包
>* 静态编译各类编码包
>* 静态编译ffmpeg


1. 各类库信息


|库名|  版本|库简介  |用途 |
|:-----:|:----:|:----:|:-----:|
|bzip2|1.0.6   |bzip2 is a freely available, patent free, high-quality data compressor||
|faac|1.28|FAAC is an MPEG-4 and MPEG-2 AAC encoder|audio |
|fdk-aac|0.1.4|fdk-aac - A standalone library of the Fraunhofer FDK AAC code from Android|audio|
|fontconfig|2.11.94|Fontconfig is a library for configuring and customizing font access|drawtext|
|freetype|2.6||drawtext|
|lame|3.99.5|Open-source mp3 encoder meant as an educational tool|audio|
|x264|latest|x264 is a free software library and application for encoding video streams into the H.264/MPEG-4 AVC compression format| video|
|libogg|1.3.1|Ogg container format libraries|audio|
|libpng|1.5.22|||
|libtheora|1.1.1|The libtheora reference implementation provides the standard encoder and decoder|video|
|libvorbis|1.3.3|The libvorbis reference implementation provides both a standard encoder and decoder under a BSD license|audio|
|libvpx|1.4.0|vp9 | video|
|opencore-amr|0.1.3|amr|audio|
|openjpeg|2.1|||
|opus|1.1|Open standard, high quality codec|audio|
|speex|1.2rc1|||
|vo-aacenc|0.1.3|Audio codecs extracted from Android Open Source Project|audio|
|vo-amrwbenc|0.1.3|Audio codecs extracted from Android Open Source Project|audio|
|x265|1.7|x265 is the leading H.265 / HEVC encoder software library. Compress video with higher quality and lower bit rates than H.264. Open source codec|video|
|xvidcore|1.3.4|Open-source compression codec based on MPEG-4 ISO format|video|
|yasm|1.3.0|Yasm is a complete rewrite of the NASM assembler under the “new” BSD License||
|zlib|1.2.8|||


2. ffmpeg信息


|版本|2.7.2|
|:--------:|:-------:|


|编译参数|--enable-postproc --enable-runtime-cpudetect --disable-ffserver --disable-debug --disable-shared --enable-static --enable-gpl --enable-nonfree --enable-libspeex --enable-version3 --enable-pthreads --enable-bzlib --enable-zlib --enable-libfreetype --enable-libfontconfig --enable-libopenjpeg --enable-libmp3lame --enable-libopus --enable-libvo_aacenc --enable-libfdk_aac --enable-libfaac --enable-libvo-amrwbenc --enable-libopencore-amrnb --enable-libopencore-amrwb --enable-libtheora --enable-libvorbis --enable-gray --enable-libx264 --enable-libx265 --enable-libxvid --enable-libvpx|
|:----:|:----:|


3. 下载地址


|文件|七牛云存储|百度云盘|
|:---:|:----:|:----:|
|ffmpeg|[下载](http://7xkru2.dl1.z1.glb.clouddn.com/ffmpeg-2.7.2)|[下载](http://pan.baidu.com/s/1i3GR0k1)|
|ffprode|[下载](http://7xkru2.dl1.z1.glb.clouddn.com/ffprobe-2.7.2)|[下载](http://pan.baidu.com/s/1kT3Wh51)|
|qt-faststart|[下载](http://7xkru2.dl1.z1.glb.clouddn.com/qt-faststart)|[下载](http://pan.baidu.com/s/1jGtiLfg)|


4. 执行信息
```python
root@stream:~# ffmpeg
ffmpeg version 2.7.2 Copyright (c) 2000-2015 the FFmpeg developers
  built with gcc 4.9.2 (Debian 4.9.2-10)
  configuration: --extra-cflags=-I/usr/local/include --extra-ldflags=-L/usr/local/lib --pkg-config-flags=--static --extra-libs='-static -lpng' --enable-postproc --enable-runtime-cpudetect --disable-ffserver --disable-debug --disable-shared --enable-static --enable-gpl --enable-nonfree --enable-libspeex --enable-version3 --enable-pthreads --enable-bzlib --enable-zlib --enable-libfreetype --enable-libfontconfig --enable-libopenjpeg --enable-libmp3lame --enable-libopus --enable-libvo_aacenc --enable-libfdk_aac --enable-libfaac --enable-libvo-amrwbenc --enable-libopencore-amrnb --enable-libopencore-amrwb --enable-libtheora --enable-libvorbis --enable-gray --enable-libx264 --enable-libx265 --enable-libxvid --enable-libvpx
  libavutil      54. 27.100 / 54. 27.100
  libavcodec     56. 41.100 / 56. 41.100
  libavformat    56. 36.100 / 56. 36.100
  libavdevice    56.  4.100 / 56.  4.100
  libavfilter     5. 16.101 /  5. 16.101
  libswscale      3.  1.101 /  3.  1.101
  libswresample   1.  2.100 /  1.  2.100
  libpostproc    53.  3.100 / 53.  3.100
Hyper fast Audio and Video encoder
usage: ffmpeg [options] [[infile options] -i infile]... {[outfile options] outfile}...

Use -h to get full help or, even better, run 'man ffmpeg'
```

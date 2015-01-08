---
layout: blog
title: debian wheezy 编译安装ffmpeg 2.x
---

debian wheezy 编译安装ffmpeg的shell 脚本

参考如下

`
#!/usr/bin/env bash
# compile ffmpeg from source code
# ffmpeg version: 2.5.1
# os: debian wheezy 7.7

# declare values
WORKDIR=`pwd`

# add source list
echo "Add source list : deb-multimedia "
echo "deb http://deb-multimedia.org wheezy main non-free" >> /etc/apt/sources.list
echo "deb-src http://deb-multimedia.org wheezy main non-free" >> /etc/apt/sources.list
echo Done

# install dependency packages
apt-get update
apt-get install -y deb-multimedia-keyring
apt-get update
apt-get install --force-yes -y subversion unzip frei0r-plugins-dev  \
  libfdk-aac-dev libmp3lame-dev libx264-dev libdirac-dev libxvidcore-dev \
  libfreetype6-dev libvorbis-dev libgsm1-dev libopencore-amrnb-dev \
  libopencore-amrwb-dev libopenjpeg-dev librtmp-dev libschroedinger-dev \
  libspeex-dev libtheora-dev libva-dev libvpx-dev libvo-amrwbenc-dev \
  libvo-aacenc-dev libaacplus-dev libbz2-dev libgnutls-dev libssl-dev \
  libopenal-dev libv4l-dev libpulse-dev libmodplug-dev libass-dev \
  libcdio-dev libcdio-cdda-dev libcdio-paranoia-dev libvdpau-dev \
  libxfixes-dev libxext-dev libbluray-dev

# compile libxavs
cd $WORKDIR
svn co https://svn.code.sf.net/p/xavs/code/trunk xavs
cd $WORKDIR/xavs
./configure --enable-shared --disable-asm
make
make install 
echo "/usr/local/lib" > /etc/ld.so.conf.d/ffmpeg
ldconfig

# compile ffmpeg
cd $WORKDIR
PROCESSORS=`cat /proc/cpuinfo | grep processor |wc -l`
VERSION=2.5.1
echo "FFmpeg version is $VERSION"
wget http://ffmpeg.org/releases/ffmpeg-$VERSION.tar.bz2
tar -jxf ffmpeg-$VERSION.tar.bz2
cd $WORKDIR/ffmpeg-$VERSION
./configure --enable-gpl --enable-nonfree --enable-postproc \
  --enable-pthreads --enable-x11grab --enable-swscale \
  --enable-version3 --enable-shared --disable-yasm --enable-filter=movie \
  --enable-frei0r  --enable-libfdk_aac --enable-libmp3lame \
  --enable-libx264 --enable-libxvid --enable-libfreetype --enable-libvorbis \
  --enable-libgsm --enable-libopencore-amrnb --enable-libopencore-amrwb \
  --enable-libopenjpeg --enable-librtmp --enable-libschroedinger \
  --enable-libspeex --enable-libtheora --enable-libvpx --enable-libvo-amrwbenc \
  --enable-libvo-aacenc --enable-libaacplus --enable-libxavs --enable-bzlib \
  --enable-openssl --enable-gnutls --enable-openal --enable-libv4l2 \
  --enable-libpulse --enable-libmodplug --enable-libass --enable-libcdio \
  --enable-vdpau --enable-libbluray

make -j $PROCESSORS
make install
# creat soft link
ln -s /usr/local/bin/ffmpeg /usr/bin/ffmpeg
ldconfig
echo "ffmpeg install success!!!"

# install qt-faststart
cd $WORKDIR/ffmpeg-$VERSION
make tools/qt-faststart
cp tools/qt-faststart /usr/bin/

# end 
echo "All done!!! Have fun!!!"
`
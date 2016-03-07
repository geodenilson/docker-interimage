FROM ubuntu:trusty
MAINTAINER Daniel Nüst <daniel.nuest@uni-muenster.de>

## (Based on https://github.com/rocker-org/rocker/blob/master/r-base/Dockerfile)
## Set a default user. Available via runtime flag `--user interimage`
## User should also have & own a home directory (for linked volumes to work properly).
RUN useradd interimage \
	&& mkdir /home/interimage \
	&& chown interimage:interimage /home/interimage \
	&& addgroup interimage staff

RUN apt-get update -qq \
		&& apt-get install -y --no-install-recommends \
	    curl \
		unzip \
		zlib1g-dev \
		lib32z1-dev \
		libgdal1-dev \
		libtiff5-dev \
		libjpeg-dev \
		libjpeg-dev \
		libgeotiff-dev \
		libterralib-dev \
		build-essential \
		gdb \
		make \
		qt4-default \
		qt4-qmake \
		# qt4-dev-tools
		&& apt-get clean

WORKDIR /home/interimage

ARG INTERIMAGE_VERSION=1.43

## Roughly following http://www.lvc.ele.puc-rio.br/projects/interimage/download/files/install_interimage_ubuntu.txt

## Get InterIMAGE source code
#RUN curl -sSL -# -O "http://www.lvc.ele.puc-rio.br/projects/interimage/download/files/InterIMAGE%20Source%20${INTERIMAGE_VERSION}.zip" \

## TO BE REMOVED: save some time by adding local file instead of downloading
COPY InterIMAGE_Source_1.43.zip InterIMAGE_Source_1.43.zip
	# unzip and get rid of filename with spaces
RUN ls && unzip InterIMAGE_Source_${INTERIMAGE_VERSION}.zip
	#&& mv InterIMAGE\ Source\ ${INTERIMAGE_VERSION} InterIMAGE_Source_${INTERIMAGE_VERSION}

## Get TerraLib source code
#RUN curl -sSL -# -O "http://www.lvc.ele.puc-rio.br/projects/interimage/download/files/terralib_cvs.zip" \

## TO BE REMOVED: save some time by adding local file instead of downloading
COPY terralib_cvs.zip terralib_cvs.zip
RUN ls && unzip terralib_cvs.zip

## Get older version source code for linux to make "ta_operators"
WORKDIR /home/interimage/linux
#RUN curl -L -# -O "http://www.lvc.ele.puc-rio.br/projects/interimage/download/files/InterIMAGE_1.27_Linux.tar.gz" \
COPY InterIMAGE_1.27_Linux.tar.gz InterIMAGE_1.27_Linux.tar.gz
RUN ls && tar xvzf InterIMAGE_1.27_Linux.tar.gz

# in addition to official instructions, copy all libraries from linux/../bin_1.27/bin to /usr/lib
WORKDIR /home/interimage/linux/bin_1.27/bin
RUN cp -P *.so* /usr/lib

# remove duplicate "liblib" in library names. (find /usr/lib -name "liblib*")
WORKDIR /usr/lib
RUN for libname in /usr/lib/liblib*; \
		do cp $libname ${libname/liblib/lib} ; \
	done

RUN ldconfig

# FIXME maybe get newer versions of libterralibtiff here?
# https://github.com/ufopterralab/terrame/blob/master/1.1.3/build/linux32/binaries.sh
WORKDIR /home/interimage/terralib_cvs/terralibmw

RUN echo -e "QMAKE_CFLAGS = -fpermissive\nQMAKE_CXXFLAGS = -fpermissive\nQMAKE_LFLAGS = -fpermissive" >> libjpeg/libjpeg.pro
RUN echo -e "QMAKE_CFLAGS = -fpermissive\nQMAKE_CXXFLAGS = -fpermissive\nQMAKE_LFLAGS = -fpermissive" >> terralib/terralib.pro

RUN qmake -r \
	&& make clean; make
#	&& mkdir /usr/lib/terralib \
#	&& cp Debug/* /usr/lib/terralib \
#	&& ldconfig


####### CONTINUE HERE ##########################################################

## Build terralibmw
#WORKDIR /home/interimage/terralib_cvs/terralibmw
#RUN qmake -r \
#	&& make clean; make \
#	&& mkdir /usr/lib/terralib \
#	&& cp Debug/* /usr/lib/terralib \
#	&& ldconfig

## Build operators from Linux download
#WORKDIR /home/interimage/linux/ta_operators/build/linux-g++
#RUN export TA_TERRALIB_PATH="/home/interimage/terralib_cvs/" \
#	&& qmake -r \
#	&& make clean; make \
#	&& cp -P ta_operators_base/Debug/* /usr/lib \
#	&& cp -P ta_operators_base/Debug/* /home/interimage/bin_1.27/bin


## ISSUE: ta_operators does not exist
#WORKDIR /home/interimage/interimage-src/ta_tdbu
#RUN export "TERRALIB_PATH=/home/interimage/interimage-src/terralib_cvs/" \
#	&& qmake -r \
#	&& make clean; make \
## does not work in ta_tdbu:
## root@8127de37b636:/home/interimage/interimage-src/ta_tdbu# qmake -r
## root@8127de37b636:/home/interimage/interimage-src/ta_tdbu# make clean; make
## rm -f tdbu.o neighbournode.o neighbournodelist.o node.o nodelist.o stack.o stackelem.o stackelemneighbournodelist.o stackelemnodelist.o  stackelemnumber.o stackelemstack.o stackelemstring.o
## rm -f *~ core *.core
## g++ -c -m64 -pipe -O2 -D_REENTRANT -Wall -W -fPIC -DQT_NO_DEBUG -DQT_QT3SUPPORT_LIB -DQT3_SUPPORT -DQT_GUI_LIB -DQT_CORE_LIB -DQT_SHARED -I/usr/share/qt4/mkspecs/linux-g++-64 -I. -I/usr/include/qt4/QtCore -I/usr/include/qt4/QtGui -I/usr/include/qt4/Qt3Support -I/usr/include/qt4 -I../../terralib_cvs/src/shapelib -I../../terralib_cvs/src/terralib/image_processing -I../../terralib_cvs/src/terralib/kernel -I../gaimage -I../qtparser -I. -o tdbu.o tdbu.cpp
## In file included from neighbournode.h:29:0,
##                  from neighbournodelist.h:27,
##                  from node.h:35,
##                  from nodelist.h:40,
##                  from tdbu.h:30,
##                  from tdbu.cpp:18:
## stack.h:40:22: fatal error: TeRaster.h: No such file or directory
##  #include <TeRaster.h>
##                       ^
## compilation terminated.

## copy the new created libraries from /ta_operators/bin/Debug to /usr/lib/
## sudo ldconfig
## copy the executables from /ta_operators/bin/Debug to /InterImage_Source/bin/

#WORKDIR /home/interimage/interimage-src
#RUN qmake -r \
#	&& make clean; make
## SAME TeRaster.h issue here!

# in addition to official instructions, copy all libraries from ~/bin_1.27/bin to /usr/lib
#RUN cp -P *.so* /usr/lib

#WORKDIR /home/interimage/InterIMAGE_Source_1.27/bin


CMD ["/bin/bash"]

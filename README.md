# A Docker image for InterIMAGE

[![DOI](https://zenodo.org/badge/doi/10.5281/zenodo.55083.svg)](http://dx.doi.org/10.5281/zenodo.55083)

These Dockerfiles are based on the InterIMAGE installation instructions on Ubuntu: http://www.lvc.ele.puc-rio.br/projects/interimage/download/files/install_interimage_ubuntu.txt

## 1.43

**Status:** The instructions linked above are not transferable directly to the latest source code.

### Build and run

To execute the container, install Docker and open a command prompt in the directory `/1.27`. Then build the container with

`docker build -t interimage/143 .`

and start it with

`docker run -it --user interimage interimage/143`


## 1.27

**Status:** Based on the existing Linux download of InterIMAGE, the Dockerfile successfully downloads and compiles InterIMAGE and the required binaries.

### Build and run

To execute the container, install Docker and open a command prompt in the directory `/1.27`. Then build the container with

`docker build -t interimage/1.27 .`

and start it with

`docker run -it --user interimage interimage/1.27`

(To remove the image directly after, add the `--rm` flag.)

This opens a bash prompt. Interimage is can be started with the following command (the image should start in the directory `cd /home/interimage/InterIMAGE_Source_1.27/bin`, if not, cd there).

```
./interimage
```


### Build and run with UI

(Based on https://hub.docker.com/r/toddstavish/orfeo_toolbox/ and https://github.com/adrienandrem/interimage-docker/blob/1.27/Makefile)

Background on GUI applications in Docker:
* http://fabiorehm.com/blog/2014/09/11/running-gui-apps-with-docker/
* http://wiki.ros.org/docker/Tutorials/GUI
* On Linux, we simply forward our X11 info with the Docker container. **This is a potential security risk** because the container can receive all X11 events that happen on the host computer desktop ([via](http://fabiorehm.com/blog/2014/09/11/running-gui-apps-with-docker/#comment-1625694548)).

```
xhost +
docker run -it --rm -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY -e uid=$(id -u) -e gid=$(id -g) -v /<path to workdir>:/home/data interimage/1.27 ./interimage
xhost -
```


### Development

* `docker build .`
* `docker run -it --rm <imageid>`


### Image content

The image has a user `interimage` which should be used to run analysis in the container if you have security considerations.

The downloads and built libraries are in the directory `home/interimage`:

```
root@a096be168745:/home/interimage# ll
total 154516
drwxr-xr-x 11 interimage interimage      4096 Feb 24 08:55 ./
drwxr-xr-x  8 root       root            4096 Feb 24 08:55 ../
drwx------ 29 interimage interimage      4096 Feb 24 08:55 InterIMAGE_Source_1.27/
drwx------  4 interimage interimage      4096 Feb 24 08:30 bin_1.27/
-rw-r--r--  1 root       root       158198233 Feb 22 16:34 interimage-src.tar.gz
drwx------  8 interimage interimage      4096 Feb 24 08:29 ta_operators/
drwx------  6 interimage interimage      4096 Feb 24 08:26 terralib_cvs/

```


## License

Copyright (C) 2016 Daniel Nüst

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.

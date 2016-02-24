# A Docker image for InterIMAGE

This Dockerfile is based on the InterIMAGE installation instructions on Ubuntu: http://www.lvc.ele.puc-rio.br/projects/interimage/download/files/install_interimage_ubuntu.txt

To run the container, install Docker and open a command prompt in the directory `/1.27`. Then build the container with

`docker build -t interimage .`

and start it with

`docker run -it --user interimage interimage`

(To remove the image directly after, add the `--rm` flag.)

This opens a bash prompt. Interimage is can be started with the following command (the image should start in the directory `cd /home/interimage/InterIMAGE_Source_1.27/bin`, if not, cd there).

```
./interimage
```


## Development

* `docker build .`
* `docker run -it --rm <imageid>`


## Image content

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


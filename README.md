This repository contains files from the ubuntu package `nvidia-l4t-jetson-multimedia-api`
that is normally installed on jetson devices at `/usr/src/jetson-multimedia-api`. 

The files now come from the version `35.3.1-20230319081403`. 

The main purpose of this repository is to ease building applications that depend on this
api, but are inside a container and need to use the helper classes provided by NVIDIA. 
Thus a set of include and source files are included and packaged into a library on a cmake 
project. 



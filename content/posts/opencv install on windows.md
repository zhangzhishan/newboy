---
title: opencv install
tag: opencv
date: 2019-03-26 09:36:55
---

##  Boost - c++ Librairies (thread and system are needed)

`sudo apt-get install libboost-all-dev`
##  Eigen 3 - Linear algebra

`sudo apt-get install libeigen3-dev`

## opencv

```sh
sudo apt-get install build-essential cmake git pkg-config
# sudo apt-get install libgtk2.0-dev libavcodec-dev libavformat-dev libswscale-dev
git clone https://github.com/Itseez/opencv.git
git checkout 3.4.4
git clone https://github.com/Itseez/opencv_contrib.git
git checkout 3.4.4
cd opencv
mkdir build
cd build
cmake ..
make -j5
sudo make install
```
## Sophus

```
cd workspace
git clone https://github.com/strasdat/Sophus.git
cd Sophus
git checkout a621ff
mkdir build
cd build
cmake ..
make
```
Note: if you encounter some lvalue errors for `unit_complex_.imag() = 0`, you may change that code to `unit_complex_.imag(0.)`;

## Fast

```
cd workspace
git clone https://github.com/uzh-rpg/fast.git
cd fast
mkdir build
cd build
cmake ..
make
```
## vikit_common

```
cd workspace
git clone https://github.com/uzh-rpg/rpg_vikit.git
cd rpg_vikit/vikit_common
mkdir build
cd build
cmake ..
make
```
in `rpg_vikit/vikit_common/CMakeLists.txt` set the flag `USE_ROS` to `FALSE`.
## SVO

```
cd workspace
git clone https://github.com/uzh-rpg/rpg_svo.git
cd rpg_svo/svo
mkdir build
cd build
cmake ..
make
```
In `svo/CMakeLists.txt` set the flag `USE_ROS` to `FALSE`.
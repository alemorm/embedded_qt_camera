# Embedded Linux Project 2

## Remote Camera using QT Visualization and a Raspberry Pi

### Preliminary Design

![Sample QT Window][sampleqt]

## Description

Testing camera and [QT] visualization remote functionality on a [Raspberry Pi 3 Model B][raspberrypi] with an [8-megapixel camera][picamera]. The Raspberry Pi takes photos of a growing houseplant over the course of a week, at a specified frequency and within a specified daylight period (e.g. 1 picture per hour from 8am to 7pm). The data is sent via TCP/IP protocol to the host device for further image processing and analysis (the file locations and timestamps are saved to a [PostgreSQL] server running on the host machine). Handcrafted features are extracted from the aggregate image data, such as brightness (mean pixel intensity) and color of the plant, which are then used to train a [linear regression model][linreg] that predicts plant growth (defined with an [image skeletonization][imageskel] method to estimate increase in plant length).

---

## Overview

The project will consist of a C++ codebase running directly on the embedded device with QT libraries linked for direct camera control. The QT widgets will be accessible remotely from a host device running (Windows 10 desktop running [WSL]). The host device will use Python and [PostgreSQL] to store the file locations to the images, acquired remotely from the embedded device. The Python [OpenCV] library will be used for the image processing and [scikit-learn] for the regression analysis.

---

## Setup

### Host Code Development

The development of the host code happens on the host device. The host device is a Windows 10 desktop running [Windows Subsystem for Linux (v1)][wsl] with [Ubuntu 18.04.4 LTS][ubuntu] and [VSCode (v1.46.1)][vscode] with the [remote SSH developmnent][sshext], [C/C++][c++], and the [Python extensions][pythonext]. The Python extension uses a [`conda (v4.8.3)`][conda] [environment] (provided).

### Embedded Code Development

The development of the embedded source code happens on the host device and the compilation happens on the Raspberry Pi.  Once the source code is updated on the host device, a [VSCode compound task][vstasks] uses [`rsync`] for synchronization and [`ssh`] commands for remote compilation using [`cmake (v3.6.2)`][cmake] on the Raspberry Pi.

### Getting Started

To replicate this project, first clone the repo on both the host and the embedded device by running:

- `git clone https://github.com/alemorm/embedded_qt_camera.git ~/`

- *Optional*: Either compile the code using the VSCode build task using `CTRL + SHIFT + B` or use the pre-compiled binaries provided (BCM2709: quad-core ARMv7).
  
  - If building the source code, make sure to install cmake (>v3.0) on the Raspberry Pi with:

    - `sudo apt-get update && sudo apt-get install cmake`
  
  - Then build the binaries with the following command:
  
    - `cd ~/embedded_qt_camera/build && cmake .. && make`

- SSH to the Raspberry Pi and run the newly created binaries by:

  - `cd ~/embedded_qt_camera/`

- On the host device, run the python script `udp_3phase` by:

  - `cd ~/embedded_qt_camera/`
  - `conda env create -f environment.yml`
  - `conda activate embeddedqt`

<!-- Links -->
[sampleqt]: img/sampleqtwindowplant.png
[qt]: https://www.qt.io/product/ui-design-tools
[raspberrypi]: https://www.raspberrypi.org/products/raspberry-pi-3-model-b/
[picamera]: https://www.raspberrypi.org/products/camera-module-v2/
[influxdb]: https://docs.influxdata.com/influxdb/v1.8/introduction/install/
[linreg]: https://en.wikipedia.org/wiki/Linear_regression
[imageskel]: https://en.wikipedia.org/wiki/Morphological_skeleton
[postgresql]: https://www.postgresql.org/about/
[opencv]: https://opencv.org/about/
[scikit-learn]: https://scikit-learn.org/stable/
[vstasks]: .vscode/tasks.json
[cmake]: https://cmake.org/cmake/help/latest/guide/tutorial/index.html
[`ssh`]: https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys
[`rsync`]: https://www.digitalocean.com/community/tutorials/how-to-use-rsync-to-sync-local-and-remote-directories-on-a-vps
[wsl]: https://docs.microsoft.com/en-us/windows/wsl/install-win10
[ubuntu]: https://www.microsoft.com/en-us/p/ubuntu-1804-lts/9n9tngvndl3q
[vscode]: https://code.visualstudio.com/
[sshext]: https://code.visualstudio.com/docs/remote/ssh
[c++]: https://code.visualstudio.com/docs/languages/cpp
[pythonext]: https://code.visualstudio.com/docs/python/python-tutorial
[conda]: https://docs.conda.io/en/latest/miniconda.html
[environment]: environment.yml

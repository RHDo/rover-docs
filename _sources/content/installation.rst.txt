.. toctree::
   :glob:

Installation Instructions
#########################

*************************************************
Before You Begin..
*************************************************

Beginning with Raspberry Pi
=================================================
Getting started instructions are here: `Quick Start Guide <https://www.raspberrypi.org/help/quick-start-guide/2/>`_

We advise you to use Raspbian Jessie or Raspbian Sketch from `this <https://www.raspberrypi.org/downloads/raspbian/>`_ website.

Enabling Bluetooth
=================================================
In some Raspbian releases, bluetooth is pre-installed. In order to check if bluetooth is working execute the following command:

.. code-block:: bash
   :linenos:

   hcitool scan
   service bluetooth status
   
In case the first command does not return with errors, that means that the bluetooth connection in Raspberry Pi is now available for use.
The second command is used for checking if the bluetooth is active.
If the first command results in an error, you can start the bluetooth service with the following commands:

.. code-block:: bash
   :linenos:

   sudo apt-get install pi-bluetooth bluez bluez-tools blueman
   sudo systemctl start bluetooth
   
.. warning:: **Use the following commands at your own RISK!!!**
   
   For older Raspbian Jessie distributions, there is a known issue of bluetooth not working properly. If you have such a problem, you can upgrade the
   distro version and use a specific kernel version which is working with bluetooth device. Following commands should be executed in order to do so:
   
   .. code-block:: bash
      :linenos:
      
      sudo apt-get update
      sudo apt-get dist-upgrade
      sudo rm /etc/udev/rules.d/99-com.rules
      sudo apt-get -o Dpkg::Options::="--force-confmiss" install --reinstall raspberrypi-sys-mods
	  
   If command above does not work, try the next one:
   
   .. code-block:: bash
      :linenos:
	  
      sudo apt-get install raspberrypi-sys-mods

   Press ``Y`` to install the mods. Then do a kernel downgrade to version 4.4.50:
   
   .. code-block:: bash
      :linenos:
	  
      sudo systemctl reboot
      sudo rpi-update 52241088c1da59a359110d39c1875cda56496764
	  
   As a final step, you should add ``btuart`` to your ``/etc/rc.local`` and reboot your Raspberry Pi.
   
   .. code-block:: bash
      :linenos:
	  
      sudo reboot
	  
   After reboot, you can test if bluetooth is working:
   
   .. code-block:: bash
      :linenos:

      hcitool scan
      service bluetooth status
	  

Enabling I2C
=================================================
Enabling I2C is explained here: `Adafruit: Configuring I2C <https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c>`_

Setting Up Camera
=================================================
How to set up the Raspberry Pi camera is explained here: `Camera configuration <https://www.raspberrypi.org/documentation/configuration/camera.md>`_

Setting Up an Access Point
=================================================
Access point setup is explained here: `Access Point Setup <https://frillip.com/using-your-raspberry-pi-3-as-a-wifi-access-point-with-hostapd/>`_.

(Please use ``192.168.168.1`` IP address for the rover.)

.. _roverappinstallation: 
*************************************************
roverapp Installation
*************************************************

Installation procedure is tested with Raspberry Pi 3 / Raspbian Jessie and Raspbian Sketch. In case of any problems, please report to one of the project maintainers: 

* Mustafa Özcelikörs <mozcelikors@gmail.com>
* Robert Höttger <robert.hoettger@fh-dortmund.de>

Requirements
=================================================
The following are the build-time dependencies required for the roverapp:

* build-essential
* cmake
* git-core
* python-pip Python package installer
* Python 2.7
* gcc

The following are the run-time dependencies used for the roverapp:

* Raspbian or similar distro that uses the Raspberry Pi metadata (meta-raspberrypi)
* Access Point setup (with IP 192.168.168.1)
* Python 2.7
* psutil Python module
* wiringPi library
* raspicam-0.1.2 or raspicam-0.1.3 library
* OpenCV 3.2.0 library
* Json-Cpp library
* bluetooth-dev, bluez, pi-bluetooth libraries
* bcm2835 library
* (modified) Adafruit_GFX and Adafruit_SSD1306 libraries
* paho.mqtt.c libraries


Manual Installation Instructions
=================================================

.. Fetch, Extract, Patch, Configure, Build, Install
.. *************************************************

Installing Dependencies
*************************************************

Installing build-time dependencies
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Installing Python 2.7
--------------------------------------------------

Raspberry Pi 3 comes with Python 2.7 installed. For other Linux distributions, it might be required to install:

.. code-block:: bash
   :linenos:

   sudo apt-get install python

   
Installing psutil
--------------------------------------------------

psutil is a performance tool library that is written in Python and that is able to provide information about GNU/Linux-based systems.
First, ``python-pip`` must be installed:

.. code-block:: bash
   :linenos:

   sudo apt-get install python-pip
   pip install psutil
   
If you already have psutil installed, you can upgrade it:

.. code-block:: bash
   :linenos:

   pip install psutil --upgrade
   
   
Installing other build-time dependencies
--------------------------------------------------

Raspberry Pi 3 comes with gcc installed. 

.. code-block:: bash
   :linenos:

   sudo apt-get install build-essential git-core cmake

Installing run-time dependencies
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Installing OpenCV
--------------------------------------------------
OpenCV installation is explained in `this link <https://docs.opencv.org/2.4/doc/tutorials/introduction/linux_install/linux_install.html>`_.


Installing wiringPi
--------------------------------------------------
Installation of wiringPi is explained in `this link <http://wiringpi.com/download-and-install/>`_.



Installing bluetooth
--------------------------------------------------
.. code-block:: bash
   :linenos:

   sudo apt-get update
   sudo apt-get install bluez pi-bluetooth bluez-tools blueman libbluetooth-dev
   
Installing jsoncpp
--------------------------------------------------
.. code-block:: bash
   :linenos:

   cd /home/pi
   git clone https://github.com/open-source-parsers/jsoncpp.git
   cd jsoncpp
   mkdir -p build
   cd build
   cmake ..
   make
   sudo make install

Installing raspicam
--------------------------------------------------

Installation of raspicam is explained in `this link <https://github.com/6by9/raspicam-0.1.3>`_.

To install raspicam, execute the following commands:

.. code-block:: bash
   :linenos:

   cd /home/pi
   git clone https://github.com/6by9/raspicam-0.1.3.git
   cd raspicam-0.1.3
   mkdir -p build
   cd build
   cmake ..
   make
   sudo make install
   
.. note:: As an alternative you can use the following git repository: `raspicam 0.1.2 <https://github.com/cedricve/raspicam.git>`_.

.. warning:: If compilation using ``make`` command fails, one should make sure ``libmmal.so`` and ``libmmal_core.so`` is located in ``/opt/vc/lib``. To install these libraries, one can do a Raspberry Pi firmware update: ``sudo rpi-update``.

Installing paho.mqtt.c libraries
--------------------------------------------------
To install paho.mqtt.c libraries for MQTT-based cloud communication, execute the following commands:

.. code-block:: bash

   git clone https://github.com/eclipse/paho.mqtt.c.git
   cd paho.mqtt.c
   make
   sudo make install


Installing roverapp
*************************************************
Roverapp's installation process is simplified by using ``CMake`` tool, a semi-automated Makefile generator for GCC compiler. The roverapp contains a ``CMakeLists.txt`` file which must be tailored after adding and linking libraries, objects, source files, and headers.
As an example, the following customly created CMakeLists.txt file eases the process when compiling roverapp, provided that dependencies are installed.

.. code-block:: cmake

	#
	# roverapp CMake file
	# https://github.com/app4mc-rover/rover-app
	#

	#
	# SETTINGS AND CMAKE CONFIG FINDING
	#

	cmake_minimum_required (VERSION 2.8.11)
	project (roverapp)
	
	# To find modules from other cmake-built projects that are searched with find_package,
	# external cmake modules need to be provided. Those are typically named as Find<package>.cmake or <package>Config.cmake.
	# Those must be added to the CMAKE_MODULE_PATH set below:
	set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_SOURCE_DIR}/ExternalCMakeModules)

	#For find_package packages, export someextlib_DIR=/path/to/..
	# Required packages
	find_package (OpenCV REQUIRED)
	find_package (raspicam REQUIRED)
	find_package (Threads)

	#Where to put binary files after "make"
	set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR}/bin)

	#Where to put library files after "make"
	set(LIBRARY_OUTPUT_PATH ${CMAKE_BINARY_DIR}/libs)

	#
	# INCLUDE DIRECTORIES
	#

	#Include directories
	include_directories (${CMAKE_SOURCE_DIR}/include)

	#Include driver headers
	include_directories (${CMAKE_SOURCE_DIR}/include/drivers/oled_drivers)

	#Include directories for external libraries
	include_directories( ${OpenCV_INCLUDE_DIRS} )
	include_directories( ${raspicam_INCLUDE_DIRS} )
	#include_directories( ${jsoncpp_INCLUDE_DIRS} )
	#include_directories( ${bluetooth_INCLUDE_DIRS} )

	#
	# LIBRARIES
	#

	#Add api
	add_library(roverapi SHARED ${CMAKE_SOURCE_DIR}/src/api/basic_psys_rover.c)

	set(CUSTOM_LIBS_DIR ${CMAKE_SOURCE_DIR}/src/libraries)

	#Add our custom libraries
	add_library (hono_interaction SHARED
	  ${CUSTOM_LIBS_DIR}/hono_interaction/hono_interaction.cpp)
	add_library (pthread_distribution SHARED
	  ${CUSTOM_LIBS_DIR}/pthread_distribution_lib/pthread_distribution.cpp)
	add_library (status_library SHARED
	  ${CUSTOM_LIBS_DIR}/status_library/status_library.cpp)
	add_library (pthread_monitoring SHARED
	  ${CUSTOM_LIBS_DIR}/pthread_monitoring/collect_thread_name.cpp)
	add_library (timing SHARED
	  ${CUSTOM_LIBS_DIR}/timing/timing.cpp)

	#Add tasks
	#Add all files to a variable ROVERAPP_TASK_FILES
	file (GLOB_RECURSE ROVERAPP_TASK_FILES ${CMAKE_SOURCE_DIR}/src/tasks/*.cpp)
	add_library (roverapptasks SHARED ${ROVERAPP_TASK_FILES})


	#Add drivers
	file (GLOB_RECURSE ROVERAPP_DRIVER_FILES
	  ${CMAKE_SOURCE_DIR}/src/drivers/oled_drivers/*.c
	  ${CMAKE_SOURCE_DIR}/src/drivers/oled_drivers/*.cpp
	)
	add_library (roverappdrivers SHARED ${ROVERAPP_DRIVER_FILES})

	#
	#  EXECUTABLE
	#

	#Add main executable
	add_executable(roverapp ${CMAKE_SOURCE_DIR}/src/roverapp.cpp)
	
	#
	#  LINKING TO ROVER API
	#

	target_link_libraries (roverapi wiringPi)
	target_link_libraries (roverapi wiringPiDev)

	target_link_libraries(roverapi hono_interaction)
	target_link_libraries(roverapi pthread_distribution)
	target_link_libraries(roverapi status_library)
	target_link_libraries(roverapi pthread_monitoring)
	target_link_libraries(roverapi timing)

	target_link_libraries (roverapi roverappdrivers)

	#
	#  LINKING TO ROVER TASKS
	#

	#Link external libraries
	# Search with sudo find / -name *libx*
	# If there is an x.so, you can link x library
	target_link_libraries (roverapptasks opencv_core)
	target_link_libraries (roverapptasks opencv_imgproc)
	target_link_libraries (roverapptasks opencv_highgui)
	target_link_libraries (roverapptasks jsoncpp)
	target_link_libraries (roverapptasks bluetooth)
	target_link_libraries (roverapptasks pthread)
	target_link_libraries (roverapptasks wiringPi)
	target_link_libraries (roverapptasks wiringPiDev)
	target_link_libraries (roverapptasks raspicam)


	#Linker actions to link our libraries to executable
	target_link_libraries(roverapptasks hono_interaction)
	target_link_libraries(roverapptasks pthread_distribution)
	target_link_libraries(roverapptasks status_library)
	target_link_libraries(roverapptasks pthread_monitoring)
	target_link_libraries(roverapptasks timing)

	#Link api
	target_link_libraries (roverapptasks roverapi)

	#Link drivers
	target_link_libraries (roverapptasks roverappdrivers)

	#
	# LINKING TO MAIN EXECUTABLE
	#

	#Link external libraries
	target_link_libraries (roverapp pthread)

	#Link api
	target_link_libraries (roverapp roverapi)

	#Linker actions to link our libraries to executable
	target_link_libraries(roverapp hono_interaction)
	target_link_libraries(roverapp pthread_distribution)
	target_link_libraries(roverapp status_library)
	target_link_libraries(roverapp pthread_monitoring)
	target_link_libraries(roverapp timing)

	#Link tasks
	target_link_libraries(roverapp roverapptasks)

	#Link drivers
	target_link_libraries (roverapp roverappdrivers)

	#
	# INSTALLING
	#

	#Install binary, shared library, static library (archives)
	install(
	  TARGETS roverapp roverapi
		roverapptasks
		roverappdrivers
		hono_interaction
		status_library
		pthread_monitoring
		timing pthread_distribution
	  PUBLIC_HEADER
	  RUNTIME DESTINATION /usr/bin
	  LIBRARY DESTINATION /usr/lib
	  ARCHIVE DESTINATION /usr/lib)

	#Copy/install files to a certain static directory
	install(FILES ${CMAKE_SOURCE_DIR}/scripts/read_core_usage.py
	  DESTINATION /opt/rover-app/scripts/)


To install roverapp, the following command should be executed:

.. code-block:: bash
   :linenos:

   git clone https://github.com/app4mc-rover/rover-app.git
   cd rover-app
   mkdir -p build
   cd build
   cmake ..
   make
   sudo make install
   
.. note:: 

   If the installation process complains about not being able to find ``FindOpenCV.cmake`` or ``OpenCVConfig.cmake``, or any ``.cmake`` file for that matter, the file must be searched and exported to the environment with commands such as the following. Then building should be done such as the following:
	
   .. code-block:: bash
	  
      sudo find / -name *OpenCVConfig.cmake*
      cd build
      sudo OpenCV_DIR=<path/to/OpenCVConfig.cmake> cmake ..
      make
      sudo make install
	  
	  
   Same goes with ``raspicam``:
   
   .. code-block:: bash
	  
      sudo find / -name *raspicamConfig.cmake*
      cd build
      sudo OpenCV_DIR=<path/to/raspicamConfig.cmake> cmake ..
      make
      sudo make install

   
If desired, the automatic installation script can be used:

.. code-block:: bash
   :linenos:

   git clone https://github.com/app4mc-rover/rover-app.git
   cd rover-app
   sudo ./install_roverapp.sh
   
After installation, roverapp can be run by the following command:

.. code-block:: bash
   :linenos:

   sudo ./roverapp
   
.. warning:: Use the following if raspicam complains about not being able to find cmake file:

   .. code-block:: bash
   
      sudo raspicam_DIR=/home/pi/raspicam-0.1.3/build/ ./install_roverapp.sh

   
.. _roverwebinstallation: 
*************************************************
roverweb Installation
*************************************************

Requirements
=================================================
The following are the build-time dependencies required for the roverweb:

* git-core

The following are the run-time used for the roverweb:

* Raspbian or similar distro that uses the Raspberry Pi metadata (meta-raspberrypi)
* Access Point setup (with IP 192.168.168.1)
* node.js with version 8.x or higher
* node.js modules: net, connect, server-static, http, socket.io, express, path, mqtt
* mjpg-streamer-experimental
* curl

Manual Installation Instructions
=================================================

Installing Dependencies
*************************************************
Installing build-time dependencies
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following are must-have build-time dependencies that are partially required by not only roverweb, but also many open-source packages.

.. code-block:: bash
   :linenos:

   sudo apt-get update
   sudo apt-get install git-core
   
Installing curl
--------------------------------------------------

.. code-block:: bash
   :linenos:

   sudo apt-get install curl
   

Installing run-time dependencies
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Installing node.js and its required modules
--------------------------------------------------

For roverweb to serve web-pages and handle communications, node.js is an important dependency. 
The following commands can be used to download and install node.js:

.. code-block:: bash
   :linenos:

   sudo apt-get update
   curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
   sudo apt install nodejs

At this point, if the following command does not result in any errors, that means that node is successfully installed:

.. code-block:: bash
   :linenos:

   node -v
   
There are also node.js modules which are required for roverweb. Those modules must be installed by running the following:

.. code-block:: bash
   :linenos:

   sudo npm install net connect serve-static http socket.io express path mqtt
   
.. warning:: In case of problems while running roverweb, do this step after navigation to project directory. 
  
Installing mjpg-streamer-experimental
--------------------------------------------------
One particular mjpg-streamer version provides streaming with Raspberry Pi. 

In order to install the module, execute the following commands:

.. code-block:: bash
   :linenos:
 
   sudo apt-get install cmake libjpeg8-dev
   
   git clone https://github.com/jacksonliam/mjpg-streamer.git

   cd mjpg-streamer-experimental
   mkdir build
   cd build
   cmake ..
   make
   sudo make install


Installing roverweb
*************************************************

Downloading (Fetching) roverweb
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash
   :linenos:

   git clone https://github.com/app4mc-rover/rover-web.git
   
After roverweb is installed, there is no need to install it to any static location. You can continue by running the server: :ref:`roverweb Getting started <roverwebstart>`
   

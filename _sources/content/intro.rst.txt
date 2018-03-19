.. toctree::
   :glob:

Introduction
#########################

.. image:: ../roverstatic/images/rover_logo.png
   :width: 25%
   :align: center
   :alt: ../roverstatic/images/rover_logo.png

*************************************************
What is rover or APP4MC-rover?
*************************************************

**Rover** (**APP4MC-rover**) is an open-source mobile robot that is designed to demonstrate the outcomes of `APP4MC <http://www.eclipse.org/app4mc>`_ and `APPSTACLE <https://itea3.org/project/appstacle.html>`_ projects.
Rover features applications and tooling required to address complex research fields such as cloud communication, open-source tooling, multi-core and cluster computing.
Rover is equipped with powerful sensors, motors, and display units to interact with the physical world. Furthermore, Rover uses `Eclipse PolarSys project <http://polarsys.org/polarsys-rover-user-story-application-platform-project-multicore-app4mc>`_ for its chassis and mechanics.

.. image:: ../roverstatic/images/rover11_2017.png
   :width: 70%
   :align: center
   :alt: ../roverstatic/images/rover11_2017.png

.. raw:: html

   <iframe width="700" height="500" src="https://www.youtube.com/embed/-YKgCpiWNCw" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

.. image:: ../roverstatic/images/rover2.jpg
   :width: 100%
   :align: center
   :alt: ../roverstatic/images/rover2.jpg

Rover software, called **roverapp** features a multi-threaded (POSIX threads or Pthreads) C/C++ implementation that runs on Linux-based embedded single board computers (such as Raspberry Pi).
Rover features countless threads dedicated to communication infrastructure, sensor driving, display unit (such as OLED displays) utilization, bluetooth communication, image processing, and behavior modes (such as Parking, Adaptive Cruise Control, Manual Driving, and Booth Modes).
It also features drivers for sensors such as magnetometers, accelerometers, various ultrasonic sensors, and camera modules. Furthermore, OLED display, buttons, a buzzer are utilized.

.. image:: ../roverstatic/images/roverwebdisplay.png
   :width: 100%
   :align: center
   :alt: ../roverstatic/images/roverwebdisplay.png

It also features a web user interface,
called **roverweb**, that is used for many purposes such as sensor data visualization, camera streaming, displaying core utilization, and web-based robot control.
Roverweb is equipped with a powerful embedded back-end that is called `node.js <https://nodejs.org/en/>`_ and a main infrastructure library called `socket.io <https://socket.io/>`_. It makes use of the created javascript programs in order to create reactive web servers that communicate with websockets.

*************************************************
rover Motivations and Research
*************************************************

Rover is used as a demonstrator for two main subjects:

.. image:: ../roverstatic/images/APP4MCLogo.png
   :width: 19%
   :align: center
   :alt: ../roverstatic/images/APP4MCLogo.png

* Model-based multi-core software development evaluation and tool support (with `APP4MC <http://www.eclipse.org/app4mc>`_ tool)
	* For this purpose, threads are designed run in a schedulable and traceable fashion (with the help of **timing** library)
	* System traces are taken in Common Trace Format using Linux' perf profiler and customly created scripts.
	* APP4MC is used for modeling and deployment outcomes

.. image:: ../roverstatic/images/APPSTACLE.png
   :width: 25%
   :align: center
   :alt: ../roverstatic/images/APPSTACLE.png

.. image:: ../roverstatic/images/KUKSA.png
   :width: 25%
   :align: center
   :alt: ../roverstatic/images/KUKSA.png

* Cloud-based communication, data infrastructure testing (for `APPSTACLE <https://itea3.org/project/appstacle.html>`_ and `Eclipse KUKSA <https://projects.eclipse.org/proposals/eclipse-kuksa>`_ projects)
	* For this purpose, a cloud instance with Eclipse Hono infrastructure is connected to using popular communication techniques such as MQTT, and REST API.
	* Eclipse Hono dashboard (using Grafana and InfluxDB) is configured.

  Our current testing ecosystem is given below:

  .. image:: ../roverstatic/images/rover_infra2_exp.png
     :width: 100%
     :align: center
     :alt: ../roverstatic/images/rover_infra2_exp.png

*************************************************
rover Infrastructure
*************************************************
A small yet crucial portion of Rover's infrastructure (especially addressing network infrastructure) is given below. Rover uses javascript object notation (JSON) format for the information that is sent and received between processes. However, it makes use of the shared memory in order to communicate between threads.

.. image:: ../roverstatic/images/complete_infra.png
   :width: 100%
   :align: center
   :alt: ../roverstatic/images/complete_infra.png

Roverapp is a C/C++ application with a single executable that makes use of its shared memory for data communication and that involves many drivers, custom libraries, external libraries, and many threads (with task functions).

Roverweb uses javascript (node.js) for its backend and javascript, jQuery, CSS, and HTML for its front-end.
Roverapp to roverweb inter-process communication is handled using NET sockets and using TCP protocol, whereas roverweb front-end and back-end is connected using websockets (with HTTP and using Socket.IO events).

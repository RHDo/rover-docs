.. toctree::
   :glob:

Starting with AGL Image - Quick Start (for Rover 0.1.0)
########################################################

Downloading the Image
==============================================================

Download the 0.1.0 Image from `this link <https://owncloud.idial.institute/s/rZ1ULqVhX9CHjrl?path=%2F>`_.

Download Etcher from `this link <https://etcher.io/>`_ and flash the image to the SD card.

Configuration Steps (One Time Only per Local Computer)
==============================================================

Installation of Node.JS, Rover Telemetry UI and Local Mosquitto Broker
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To demonstrate the rover, you'll need Rover Telemetry UI.

First install node.js with the help of `http://nodesource.com/blog/installing-node-js-tutorial-ubuntu/ <http://nodesource.com/blog/installing-node-js-tutorial-ubuntu/>`_ for Ubuntu 16.04.

On your local machine, to download Rover Telemetry UI:

.. code-block:: javascript
   :linenos:

   git clone https://github.com/app4mc-rover/rover-telemetry-ui.git

On your local machine, to download dependencies (If you don't have node.js installed, first install node.js):

.. code-block:: javascript
   :linenos:

   cd rover-telemetry-ui
   sudo npm install net connect serve-static http socket.io express path mqtt


If you don't have a mosquitto broker, you can install one to your local machine by using the following. If you have, please skip the following command.

	.. code-block:: bash
	   :linenos:

	   sudo apt-get install mosquitto


Running Steps
==============================================================

Starting Rover Telemetry UI
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

On your local machine, to run the Rover Telemetry UI server:

.. code-block:: javascript
   :linenos:

	   cd scripts/
	   sudo node start_rovertelemetryui.js

Configuring rover.conf
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Now, configure rover.conf on your local machine and copy it to the rover:

	.. code-block:: bash
	   :linenos:

	   cd ~
	   wget 'https://raw.githubusercontent.com/app4mc-rover/rover-app/0.1.1/samples/rover.conf.sample'
	   mv rover.conf.sample rover.conf
	   nano rover.conf

	.. code-block:: ini
	   :linenos:

	   # Configuration file for roverapp.
	   # Place in /etc/rover.conf
	   ROVER_IDENTITY_C=1
	   MQTT_BROKER_C=127.0.0.1
	   MQTT_BROKER_PORT_C=1883
	   ROVER_MQTT_QOS_C=0
	   MQTT_USERNAME_C=sensor1@DEFAULT_TENANT
	   MQTT_PASSWORD_C=hono-secret
	   USE_GROOVE_SENSOR_C=0
	   USE_REDIRECTED_TOPICS_C=0
	   HONO_HTTP_HOST_C=idial.institute
	   HONO_HTTP_PORT_C=8080
	   HONO_HTTP_TENANT_NAME_C=DEFAULT_TENANT
	   HONO_HTTP_DEVICE_ID_C=4711
	   HONO_HTTP_REGISTER_PORT_C=28080
	   HONO_HTTP_USERNAME_C=sensor1
	   HONO_HTTP_PASSWORD_C=hono-secret

The configuration file should look like the one above. It is space sensitive so be sure not to introduce any unnecessary whitespaces or blank characters to the format. You should configure ``MQTT_BROKER_C`` with the exact hostname of the MQTT broker that you will use.

If you have chosen to use a mosquitto broker on your local computer, you should configure ``MQTT_BROKER_C`` with your IP address. On a Linux machine you can find out your IP address with:

	.. code-block:: bash
	   :linenos:

	   ifconfig

For testing only, you don't have change the ``MQTT_BROKER_PORT_C``, ``MQTT_USERNAME_C``, ``MQTT_PASSWORD_C`` values.

If you are using a Groove sensor, change ``USE_GROOVE_SENSOR_C`` to 1, default is 0.

If you are planning to redirect messages from Eclipse Hono, change ``USE_REDIRECTED_TOPICS_C`` to 1, default is 0.

Now, copy the configured rover.conf to the rover. For this you'll need to boot your rover with the recently flashed SD card.

Copy the configuration file from your PC (fill in the <rover-ip-address>):

	.. code-block:: bash
	   :linenos:

	   sudo apt-get install rsync
	   rsync -r rover.conf root@<rover-ip-address>:/etc/rover.conf


(If you didn't already) SSH into your rover by using (fill in the <rover-ip-address> ):

If you dont know the rover's IP address you can either use Nmap on Linux (i.e. sudo nmap -sn 192.168.1.0/24), or Advanced IP Scanner on Windows.

	.. code-block:: bash
	   :linenos:

	   ssh root@<rover-ip-address>

Killing the previously built roverapp
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Once SSHed, kill the existing roverapp:

	.. code-block:: bash
	   :linenos:

	   ps -aux | grep roverapp

Look for the PID value in the first column and in the row where /usr/sbin/roverapp executable is located, then replace this with <PID> below:

	.. code-block:: bash
	   :linenos:

	   kill -9 <PID>

Starting the roverapp with an Eclipse Che-built executable
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If previously deployed, Eclipse Che-built executable is located at /projects/rover-app/build/bin/. Simply type the following to start the app:

.. code-block:: bash
   :linenos:

   /projects/rover-app/build/bin/roverapp

Starting the roverapp with a back-up version 0.1.1
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If previously deployed, type the following to run the backup version:

	.. code-block:: bash
	   :linenos:

	   /home/mozcelikors/qtCreatorWorkspace/roverapp/build/bin/roverapp


.. note::

	If you dont have a backup version 0.1.1 pre-built executable, you can download the build directory from `this link <https://owncloud.idial.institute/s/wodPfDmgtRv47Gk>`_.
	After downloading, this executable should be copied exactly at ``/home/mozcelikors/qtCreatorWorkspace/roverapp/build/`` in AGL image.
	This is a workaround for those who would like to only test with the rover and not develop on it.

	To do this, unzip the file to a directory named ``build``. On your local machine, copy its contents to rover using:

	.. code-block:: bash
	   :linenos:

	   rsync -r build/* root@<rover-ip-address>:/home/mozcelikors/qtCreatorWorkspace/roverapp/build/

Final steps for demonstration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Go to your web browser and find the Rover Telemetry UI at ``http://<your host address>:5055/rovertelemetryui.html``.

In the telemetry UI, you have to enter broker address starting with ``tcp://``, If you are using a local mosquitto server this would be ``tcp://<your-ip-address>``.
If you configured credentials, you can also enter them. Default is without any credentials. After specifying the rover ID as it is in the rover.conf, hit Connect.


If you would like to also test rover-web, simply go to ``http://<rover-ip-address>:5500/roverweb.html`` from your local machine.


(Optional) Connecting to a Wifi on Rover AGL Image
==============================================================

Until this point, we had the assumption that you SSHed into rover using an ethernet cable. If you want to avoid that and want to able to connect to rover with wifi you'll have to use ``connmanctl``.

Now, by using your ethernet cable, SSH into the rover like you normally would and type the following:

	.. code-block:: bash
	   :linenos:

	   connmanctl
	   agent on
	   services

You should now be able to see the available wifi networks around rover. Copy the psk key of the network you want to connect to (i.e. ``wifi_b827eb861868_6f322d574c414e3837_managed_psk``). Then, type the following by replacing <copied-psk-key> with the copied value:

	.. code-block:: bash
	   :linenos:

	   connect <copied-psk-key>

You'll be asked a Passphrase. Enter it and wait for a message. If properly connected, you'll be notified that you're now connected. Then type:

	.. code-block:: bash
	   :linenos:

	   quit

If you weren't able to connect, Repeat this section one more time.

Now, exit the SSH session and plug off the ethernet cable.
Obtain the new IP address of your rover and you can use this new IP address from now on for SSHing and controlling the rover.


Killing Roverapp
==============================================================

Press Ctrl + C a few times


(Optional) Developing with Eclipse Che
==============================================================

If you want to develop on the rover using Eclipse Che, go to the rover-connected Eclipse Che instance from your browser. An example instance is currently located at `IDiAL Eclipse Che with CppAglDev Factory <http://idial.institute:38080/dashboard/#/load-factory/?url=https://github.com/mozcelikors/CppAglDev>`_.

To start developing with the rover, you need to obtain the IP address of the rover, which should be in Eclipse Che instance network. Then you need to configure the created workspace by changing the TARGET_IP environment variable from workspace settings. After restarting the workspace, the workspace will pull the master repository and set up the commands to Configure, Build, Deploy and Run the roverapp.

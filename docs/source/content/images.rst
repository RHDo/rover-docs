.. toctree::
   :glob:

Ready Images
########################################

Automotive Grade Linux (AGL) Image for Rover 0.1.0
===================================================

Download `AGL Image Rover 0.1.0 <https://owncloud.idial.institute/s/rZ1ULqVhX9CHjrl?path=%2F>`_.

Use `this link <https://app4mc-rover.github.io/rover-docs/content/aglimg.html>`_ to start with this AGL Image.


Raspbian Jessie Image
========================================

Download `Raspbian Jessie Image <https://owncloud.idial.institute/s/gLp5ZAd7SKz3bYM>`_.

	* Includes dependencies and current rover-app, rover-telemetry-ui, and rover-web repositories
	* Image Updated: 10.01.2017
	
To install the latest roverapp
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash
   :linenos:

   cd ~/rover-app
   git pull
   git checkout master
   sudo ./install_roverapp.sh

To start roverapp
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash
   :linenos:

   cd ~/rover-app/build/bin
   sudo ./roverapp
   
Using rover-telemetry-ui
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. note:: Project is hosted in the repository: `https://github.com/app4mc-rover/rover-telemetry-ui.git <https://github.com/app4mc-rover/rover-telemetry-ui.git>`_.

To download:

.. code-block:: javascript
   :linenos:
   
   git clone https://github.com/app4mc-rover/rover-telemetry-ui.git
   
To download dependencies (If you don't have node.js installed, first install node.js):

.. code-block:: javascript
   :linenos:
   
   cd rover-telemetry-ui
   sudo npm install net connect serve-static http socket.io express path mqtt

To run the server:

.. code-block:: javascript
   :linenos:   
   
   cd scripts/
   sudo node start_rovertelemetryui.js
   
Finally, go to your web browser and find the page at ``http://<your host address>:5055/rovertelemetryui.html``.
   
To download latest roverweb
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash
   :linenos:

   cd ~/rover-web
   git pull
   git checkout master
   cd rover-web
   sudo npm install net connect serve-static http socket.io express path mqtt
   
To start roverweb
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash
   :linenos:

   cd ~/rover-web/scripts/nodejs
   sudo node start_roverweb.js
   
.. warning:: Be sure to change the folder to ``cd <your/roverweb/root/path>/scripts/nodejs/`` before executing the script, since it'll use default directory for hosting.

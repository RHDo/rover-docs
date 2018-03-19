.. toctree::
   :glob:

Additional Features
###################

****************************************
Tracing with Rover
****************************************

.. only:: builder_html

   See :download:`Rover Tracing Manual <../roverstatic/pdfs/RoverTracing.pdf>`.
   
   
****************************************
A Short Tutorial on MQTT
****************************************
Installing MQTT-Broker (A server that is able to host topics, and MQTT v3.1 protocol):

.. code-block:: bash

   sudo apt-get install mosquitto 

Installing MQTT-Client implementations for subscription and publishing:

.. code-block:: bash

   sudo apt-get install mosquitto-clients

Starting the MQTT-Broker:

.. code-block:: bash

   mosquitto -p 1883

Subscribing to a topic:

.. code-block:: bash

   mosquitto_sub -v -t 'rover/RoverDriving/control/1'

Publishing to a topic:

.. code-block:: bash

   mosquitto_pub  -t 'rover/RoverDriving/control/1' -m 'my message'
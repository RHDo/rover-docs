.. toctree::
   :glob:

Rover Software: roverapp
#########################

.. image:: ../roverstatic/images/roverapp_logo.png
   :width: 40%
   :align: center
   :alt: ../roverstatic/images/roverapp_logo.png
   
   
*************************************************
What is roverapp?
*************************************************

**Roverapp** is an open-source C/C++ project for single board Linux computers (Raspberry Pi), especially designed for **rover**.
It features multi-threaded schedulable and traceable software under GNU/Linux environment. For multi-threading, POSIX threads (Pthreads)
and its synchronization implementations such as mutexes are widely used. 

**Roverapp** complete feature list is given below:

* Multi-threaded, schedulable and traceable embedded software.
* Cloud communication to `Hono 0.5-M9 <https://www.eclipse.org/hono/api/>`_ infrastructure using REST API, using customly created **hono_interaction** library.
* Utilized drivers for Linux modules such as bluetooth (bluetooth-dev).
* I2C drivers and applications (threads) for OLED display, SRF02 ultrasonic sensor, HMC5883L magnetometer, GY-521Y accelerometer, etc.
* Temperature and humidity measurement using DHT22 sensor.
* Reactive implementations for TCP socket server and TCP socket client, with proper JSON formatted data for communication.
* OLED display application that is able to display bluetooth, Hono cloud, ethernet, wireless interface, and internet with the help of **status_library** library.
* OpenCV 2.4.9 utilization and image processing application (currently Traffic cone detection).
* SHARP Analog Proximity measurement sensor interfacing and implementations.
* SRF-02, HCSR-04, and Grove Ultrasonic sensor interfacing and implementations.
* Motor driving implementations.
* Timing measurement implementations with the help of **timing** library.
* CPU core utilization measurement implementation.
* Adaptive Cruise Control behavior implementation.
* Parking behavior implementation.
* Booth mode implementations.
* Implementations for bluetooth-based driving from Android phones.

*************************************************
roverapp Infrastructure
*************************************************
.. image:: ../roverstatic/images/roverapp_infra.png
   :width: 100%
   :align: center
   :alt: ../roverstatic/images/roverapp_infra.png

*************************************************
Downloading roverapp
*************************************************

.. note:: Rover-app source code is maintained under the following repository:

          `https://github.com/app4mc-rover/rover-app <https://github.com/app4mc-rover/rover-app>`_
		  
*************************************************
Getting started with roverapp
*************************************************
Installation
=================================================
.. note:: In order to see how roverapp is installed and compiled, please see :ref:`roverapp Installation <roverappinstallation>` section.

Running roverapp
=================================================

After installation, roverapp can be run by the following command from project root:

.. code-block:: bash
   :linenos:

   sudo ./roverapp

*************************************************
roverapp Complete Reference
*************************************************

Drivers
=================================================
OLED Libraries
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Created Libraries
=================================================
status_library
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
hono_interaction
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The current release covers communication with Eclipse Hono version 0.5-M9.

Registering a Device
-------------------------------------------------
Arguments
&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
In order to register a device to the Eclipse Hono instance, ``registerDeviceToHonoInstance`` should be used. This function is defined as follows:

.. code-block:: c++
   :linenos:
   
   int registerDeviceToHonoInstance (char * host_name, int port, char * tenant_name, char * device_id)

The function takes the following arguments:

* Host name to connect (char \*)
* Port to connect (int)
* Tenant name (char \*)
* Device ID (char \*)

Operation
&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
This function constructs a curl call on the system by constructing a string such as the following:

.. code-block:: python
   :linenos:

   curl -X POST -i -H 'Content-Type: application/json' -d '{"device-id": "4711"}' http://idial.institute:28080/registration/DEFAULT_TENANT
   
Afterwards, returning pipe content is read and parsed to decide whether the call is accepted or what kind of error has occurred.

Sending Telemetry Data to Eclipse Hono Instance
--------------------------------------------------
Arguments
&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
In order to send telemetry data to the Eclipse Hono instance, ``sendTelemetryDataToHonoInstance`` should be used. This function is defined as follows:

.. code-block:: c++
   :linenos:
   
   int sendTelemetryDataToHonoInstance (char * host_name, int port, char * tenant_name, char * device_id, char * user, char * password, char * field, double value)

The function takes the following arguments:

* Host name to connect (char \*)
* Port to connect (int)
* Tenant name (char \*)
* Device ID (char \*)
* Username (char \*)
* Password (char \*)
* Field name (char \*)
* Value (double)

Operation
&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
This function constructs a curl call on the system by constructing a string such as the following:

.. code-block:: bash
   :linenos:

   curl -X POST -i -u sensor1@DEFAULT_TENANT:hono-secret -H 'Content-Type: application/json' --data-binary '{"temp": 5}' http://idial.institute:8080/telemetry
   
Afterwards, returning pipe content is read and parsed to decide whether the call is accepted or what kind of error has occurred.


Sending Event Data to Eclipse Hono Instance
--------------------------------------------------
Arguments
&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
In order to send event data to the Eclipse Hono instance, ``sendEventDataToHonoInstance`` should be used. This function is defined as follows:

.. code-block:: c++
   :linenos:
   
   int sendEventDataToHonoInstance (char * host_name, int port, char * tenant_name, char * device_id, char * user, char * password, char * field, double value)

The function takes the following arguments:

* Host name to connect (char \*)
* Port to connect (int)
* Tenant name (char \*)
* Device ID (char \*)
* Username (char \*)
* Password (char \*)
* Field name (char \*)
* Value (double)

Operation
&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
This function constructs a curl call on the system by constructing a string such as the following:

.. code-block:: bash
   :linenos:

   curl -X POST -i -u sensor1@DEFAULT_TENANT:hono-secret -H 'Content-Type: application/json' --data-binary '{"temp": 5}' http://idial.institute:8080/event
   
Afterwards, returning pipe content is read and parsed to decide whether the call is accepted or what kind of error has occurred.
 
Handling REST API /HTTP Responses and Status Codes
--------------------------------------------------
In order to handle the response codes of HTTP requests, ``handleCode`` function is developed:

.. code-block:: c++
   :linenos:
   
	int handleCode(int code)
	
The commonly occuring HTTP responses are parsed and displayed in this library within ``handleCode`` function.

============== ====================== ======== =======================================================
Error Code     Meaning                Status   Explanation in Hono Terminology
============== ====================== ======== =======================================================
200	           OK                     1        Device registration is successful
201	           Created                1        Device registration is successful
202            Accepted               1        Telemetry data is accepted at Hono instance
200..209                              1	        
400	           Bad Request            0         
403            Forbidden              0         
409            Conflict               0        Device is already registered
400..409                              0         
503            Service Unavailable    0        There is no consumer running. Please create a consumer
============== ====================== ======== =======================================================

In the library, the meanings of commonlu occurring error codes are considered and status codes are generated.
Status or exit code is what the functions in this library returns as an integer.

The following (non-UNIX) convention is adapted:

* 1 → Successful exit
* 0 → Non-successful exit

The status value can be used to check if functions succeed. Please see `Example Usage` section.

Debugging
--------------------------------------------------
The header file of the library contains two compiler directives for easy debugging of console output.

.. code-block:: c++
   :linenos:
   
   #define DEBUG_HTTP_RESPONSE 1
   #define DEBUG_DEVICE_REGISTRATION 1
   
One can either define or comment/undefine these compiler directives.
The first compiler directive is used for displaying the strings on console such as:

* Constructed curl command
* Parsed HTTP response code
* The entire pipe that is returned

The second compiler directive is used for displaying the same information, but only in the device registration function.
Ideally, since device is registered only once but the data is send constantly, it is advised to see this information at the device registration function, but not for the other functions, to prevent the console overhead.
Due to the aforementioned reason, it is advised to keep this part as follows:

.. code-block:: c++
   :linenos:
   
   //#define DEBUG_HTTP_RESPONSE 1
   #define DEBUG_DEVICE_REGISTRATION 1
   
But uncomment the first line only when it is suspected that something regarding cloud connection is failing.

When there is no internet connection with ``DEBUG_HTTP_RESPONSE`` defined, a message such as the following is displayed: ``Code: 0``.

Example Usage
-------------------------------------------------

.. code-block:: c++
   
	#include <stdio.h>
	#include <stdlib.h>
	#include "hono_interaction.h"
	int main ()
	{
		  int status;
		  double bearing_val;
		  status = registerDeviceToHonoInstance("idial.institute",28080,"DEFAULT_TENANT", "roverBearing");
		  if (status == 0)  // If DEBUG_HTTP_RESPONSE is commented, you can manually handle errors by using the status/exit code
		  {
				fprintf (stderr, "Device is not registered");
		  }

		  while (1)
		  {
				bearing_val = 0.5001;
				status = sendTelemetryDataToHonoInstance("idial.institute",8080,"DEFAULT_TENANT", "4711","roverBearing", bearing_shared);
				if (status == 0)  // If DEBUG_HTTP_RESPONSE is commented, you can manually handle errors by using the status/exit code
				{
					  fprintf (stderr, "Data is not sent");
				}
		  }  
	}

Resources
-------------------------------------------------
The material provided for hono_interaction uses Hono API, which is provided here: `Hono API <https://www.eclipse.org/hono/api/>`_.

pthread_monitoring
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
pthread_distribution_lib
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
timing
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Tasks and Threads
=================================================
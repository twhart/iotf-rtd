Python Client Library - Introduction
============================================

Use the Python module for interacting with the `IBM Internet of Things Foundation <https://internetofthings.ibmcloud.com>`__ and to automate commands using Python 3.4 or 2.7. The module can be used to simplify interactions with the IBM Internet of Things Foundation. The following libraries contain instructions and guidance on using the python ibmiotf module to interact with devices and applications within your organizations.

-  `Python 3.4 <https://www.python.org/downloads/release/python-343/>`__
-  `Python 2.7 <https://www.python.org/downloads/release/python-279/>`__

Note: Support for MQTT over SSL requires at least Python v2.7.9 or v3.4, and openssl v1.0.1

This client library is divided into two sections, both included within the library. The Devices section contains information on how devices publish events and handle commands using the Python ibmiotf module, and the Applications section contains information on how applications can use the ibmiotf module to interact with devices.

Dependencies
-------------------------------------------------------------------------------

-  `paho-mqtt <https://pypi.python.org/pypi/paho-mqtt>`__ - provides a client class which enable applications to connect to an MQTT broker
-  `iso8601 <https://pypi.python.org/pypi/iso8601>`__ - parses the most common forms of ISO 8601 date strings into datetime objects
-  `pytz <https://pypi.python.org/pypi/pytz>`__ - allows accurate and cross platform timezone calculations
-  `requests <https://pypi.python.org/pypi/requests>`__ - an HTTP library

----


Installation
------------
To install the latest version of the library with pip, enter the following code in your command line.
::

    [root@localhost ~]# pip install ibmiotf

Uninstall
---------
Uninstalling the module is simple, enter the following code in the command line. 

::

    [root@localhost ~]# pip uninstall ibmiotf


Migrating from v0.0.x to v0.1.x
-------------------------------
There is a significant change between the 0.0.x releases and 0.1.x that will require changes to client code.  Now that the library properly supports multiple message formats you will want to update calls to deviceClient.publishEvent, appClient.publishEvent and appClient.publishCommand to also supply the desired message format.

Sample code v0.0.9:

.. code:: python

    deviceOptions = {"org": organization, "type": deviceType, "id": deviceId, "auth-method": authMethod, "auth-token": authToken}
    deviceCli = ibmiotf.device.Client(deviceOptions)
    myData = { 'hello' : 'world', 'x' : x}
    deviceCli.publishEvent(event="greeting", data=myData)

Sample code v0.1.1:

.. code:: python

    deviceOptions = {"org": organization, "type": deviceType, "id": deviceId, "auth-method": authMethod, "auth-token": authToken}
    deviceCli = ibmiotf.device.Client(deviceOptions)
    myData = { 'hello' : 'world', 'x' : x}
    deviceCli.publishEvent(event="greeting", msgFormat="json", data=myData)

Also, as part of this change, events and commands sent as format "json"
will not be assumed to meet the `IoTF JSON Payload
Specification <../messaging/payload.html#iotf-json-payload-specification>`__.
The default client behaviour will be to parse commands and events with
format "json" as a generic JSON object only. Only messages sent as
format "json-iotf" will default to being decoded in this specification.
This can be easily changed with the following code.

.. code:: python

    import ibmiotf.device
    from ibmiotf.codecs import jsonIotfCodec

    deviceOptions = {"org": organization, "type": deviceType, "id": deviceId, "auth-method": authMethod, "auth-token": authToken}
    deviceCli = ibmiotf.device.Client(deviceOptions)
    # Revert to v0.0.x parsing for json messages -- assume all JSON events and commands use the IoTF JSON payload specification
    deviceCli.setMessageEncoderModule('json', jsonIotfCodec) 


Documentation
-------------
* `Device Client <../libraries/python_cli_for_devices.html>`__
* `Application Client <../libraries/python_cli_for_apps.html>`__

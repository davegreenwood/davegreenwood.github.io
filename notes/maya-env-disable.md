# Improve Maya Startup and Shutdown Times

The Maya.env file can be used to disable some processes that might impact startup and shutdown times.

* Customer involvement program, which tries to connect to Amazon.
* App licensing and in product messaging.
* Customer Error Reporting.

Add the following lines to Maya.env

    MAYA_DISABLE_CIP=1
    MAYA_DISABLE_CLIC_IPM=1
    MAYA_DISABLE_CER=1

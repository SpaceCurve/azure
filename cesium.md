Cesium with Data Streaming
==========================

This document describes code published at [SpaceCurve/cesium](https://github.com/SpaceCurve/cesium) and included in the 
SpaceCurve QuickStart Virtual Machine on Azure.

This Cesium build adds a **GeometryBuffer** object you can use to queue points, lines, 
and polygons onto a scene, and flush these visual elements to the GPU in a single operation. If you use a timer to
regularly flush queued elements, data can be rendered as it arrives.

These instructions describe the steps you will take to use this modified Cesium build with sample data on the SpaceCurve QuickStart Virtual Machine.

Dependencies
------------

* Before following these instructions, be sure you installed sample data by
following the instructions in the **Add Sample Data** section of 
[SpaceCurve QuickStart Virtual Machine on Azure](README.md).

* This example requires a browser that includes WebGL. A browser that runs [Cesium Demos](http://cesiumjs.org/) should also run this example.

Run Node.js to host Cesium
--------------------------

Our Virtual Machine includes Node.js to run Cesium. To start the Node.js instance, 
create an ssh terminal window using the instructions in [SpaceCurve QuickStart Virtual Machine on Azure](README.md).
Then enter these commands in the ssh terminal window:

        cd ~/cesium
        node server.js 5555

Run Example in Client Browser
-----------------------------

Now you can use a browser-based example in the SpaceCurve VM to show data as it arrives in the browser.

In this example, the SpaceCurve VM operates as a server. Follow these steps to view the example:

1. Get the DNS NAME of your Azure VM. You can find this address in the dashboard for this VM.
2. In a browser on your host computer, paste the DNS NAME into the location bar, add **:5555** to the address, and press the **return** key. 
The address in the browser will look *similar* to this:

        spacecurve-123-DNS-address.cloudapp.net:5555

### Query data in the Example

This example lets you query data from the SpaceCurve System. If the data contains geospatial properties, this example will render its points or polygons onto the map. This example uses a timer to flush the GeometryBuffer frequently, while data streams from the SpaceCurve System into the browser.

Enter one of these queries into the **Manual Query** field. You will see points or polygons appear on the map of the United States. Note that visual rendering begins before all of the data has arrived.

    select * from schema.us_counties;
    select * FROM schema.us_cities where "properties"."NAME" LIKE 'S%' ;   
    select * from schema.us_counties where "properties"."POP2000" > 10000 ;  

This example shows points as dots, and shades polygons according to their POP2000 property value. In this example, you can zoom into and out of the map using the scroll wheel on the mouse.

SpaceCurve QuickStart Virtual Machine on Azure Marketplace
==================================================

The SpaceCurve QuickStart Virtual Machine on Azure Marketplace lets you try an installation of the
SpaceCurve System on the Microsoft Azure cloud application platform. 
You can load sample data, use the SpaceCurve SQL shell to
query data, and use browser-based example applications to query geographic data
and view results on an interactive world map.

The SpaceCurve System is a distributed and parallel geospatial data and
analytics platform. This example implementation runs a Master process, a Front
process, and two Worker processes on a single computer. SpaceCurve installations
can reach petabyte scales by adding both processes and computers to meet
increasing throughput demands.

Follow the instructions in this document to set up the QuickStart VM and sample
data.

As you launch your VM on Azure Marketplace, keep these details in mind:
 * Remember that a VM of this size accrues allocation costs to your account when your VM is running! To save money, turn off the VM when not actively using it.
 * You must use the **`azureuser`** user name when you launch your VM. Don't change this default user name!


Copyright and EULA
--------------------

By accessing the SpaceCurve QuickStart VM you indicate your agreement to the
conditions of use specified in the `COPYRIGHT.txt` file and in the EULA
agreement. 

http://www.spacecurve.com/amember/signup/aXces2azureMP


### Connect to your VM using ssh

1. Find the DNS address of your VM on Azure. You can find this address after the deployment completes, in the Azure Management Console under VIRTUAL MACHINES as the DNS NAME for the running VM.
2. In your Terminal window, launch ssh with a call *similar to* this bash command:  
    `ssh -i ~/key.pem azureuser@spacecurve-123-DNS-address.cloudapp.net`  
    (You might use a password rather than a .pem file. Be sure to add your DNS address (not ours, shown here) after **azureuser@**.)

Use the ssh session in your Terminal window to control your virtual machine and complete the instructions in this document.

**Hint:** You will use an additional ssh session to run the Node.js web server for the Cesium example application. You can create an additional ssh session using the same command above in another Terminal window, and set it aside for later.

Run SpaceCurve System
---------

In your ssh Terminal window, enter this command to confirm SpaceCurve System is running:

`scctl status`

You should see four processes with **running** status. If you do not, wait a bit and try the command again.


Initialize the sample data database instance
---------

Enter these commands in your ssh Terminal window to initialize your database instance.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
cd ~/VM/scripts
./createdb.sh
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



Add Sample Data
---------------

Follow these three steps to load any dataset into SpaceCurve:

1.  Transform the dataset to GeoJSON using GDAL.

2.  Run the schema discovery script to create a DDL for the dataset.

3.  Upload the dataset to SpaceCurve using an HTTP client such as cURL.

The virtual machine contains sample datasets in both CSV and Shapefile
formats. Follow these instructions to load sample data
into the SpaceCurve System.

### Add Census Data

In the ssh Terminal window, enter these commands to load the census dataset into the
SpaceCurve System database:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
cd ~/VM/datasets/census
sh scripts/1_transform.sh
sh scripts/2_schema.sh > /dev/null # Runs a min+ w/ no output
sh scripts/3_load.sh               # Runs a min or more
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can test this dataset by entering a query in the SpaceCurve SQL shell.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
scctl shell -n ArcGIS      # runs the SpaceCurve SQL shell
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Enter this query in the SpaceCurve SQL shell to show all cities within 9
kilometers of Seattle:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
SELECT * FROM schema.us_cities WHERE "geometry".ST_Distance(ST_Point(-122.3, 47.6)) <= 9000.0;
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A result set of JSON data appears. This data desribes the cities around (and
including) Seattle. These include Bellevue, Mercer Island, and others.

Type `q` to exit the data view in the shell, and type `\q` to exit the shell
application.

### Use the Scripts with Your Data

You can use the three scripts to prepare and load your data into the SpaceCurve
System. Look at the script code to understand more about the data preparation
and load process. Be sure to examine the generated DDL (in the `.sql` file)
which is produced by the SchemaDiscovery tool in the **2\_schema.sh** script.
You can modify the schema in the `.sql` file before loading the data into the
SpaceCurve System using the **3\_load.sh** script.

**Note:** The SpaceCurve QuickStart VM is not a secure enviroment, so don't load sensitive data onto this VM.

Start Over?
-----------

You might wish to reset this installation of the SpaceCurve System. The simplest
way to start over form scratch is to shutdown SpaceCurve, erase its resources
(log files, volume files), and reinitialize the server. You can reset your installation
by running the `install_cluster.sh` command.

    cd /tmp/prep
    sudo ./install_cluster.sh

Next Steps
----------

Now that you have a running installation of the SpaceCurve System that includes
sample data, follow the instructions in [Cesium with Data Streaming][2] to install and use an example application with Cesium.

You can view the SpaceCurve Platform documentation when you register with SpaceCurve at [spacecurve.com/docs](http://www.spacecurve.com/docs). The documentation includes detailed
information about the SpaceCurve System, including its query language.


[2]: <cesium.md>

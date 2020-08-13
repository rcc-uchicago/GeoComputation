# Geocomputation Workshop

This workshop will introduce attendees to performing Geocomputation analysis using a High-Performance Computing (HPC) cluster. Methods for parallel computing will be presented in the context of geospatial analysis. Participants will be introduced to geospatial tools available on the midway cluster. Most of the hands-on training could be done on any operating system; however, some parts of the hands-on will be demonstrated using the Midway2 cluster.

(i) introduction to single-threaded and multithreaded geospatial tools such as GDAL/OGR, arcpy, and simple features (SF), raster, and stars packages in R.

(ii) implicit and explicit parallelization

(iii) introduction to Slurm

(iv) writing simple Bash scripts for raster and vector processing

**Prerequisites:** Basic knowledge of Geographic Information Systems and previous experience of working with the command line is assumed.

---

All the exercise could be replicated on midway cluster. Anyway, if you don't have access to it, follow the exercise, try to understand the concept, and then you can replicate in your computer.

## **Part 1: introduction to single-threaded and multithreaded geospatial tools such as GDAL/OGR, arcpy, and simple features (SF), raster, and stars packages in R.**

Geo-computation is the computation and manipulation of geographic data to answer different kinds of research questions. This data is becoming bigger and bigger, larger and larger, as you saw in the last ten years. So you really need powerful tools to be able to compute large processing analysis. You also need a huge computer-- so a supercomputer that we are using at UChicago. So the combination of powerful tools and powerful computers allows you to perform extensive analysis. Nonetheless, these tools, especially with GDAL, R, and python, can be used also on your laptop.

**Linux shell:** If you want to learn about the shell you should go to the [software carpentry website](https://swcarpentry.github.io/shell-novice/). What you will find here are the commands that are essential for working with GDAL.

*Shell prompt in native Linux*

`$`

![Geocomputation%20Workshop%207775d197cbc646d697b387c8e4d55b32/Untitled.png](Geocomputation%20Workshop%207775d197cbc646d697b387c8e4d55b32/Untitled.png)

### Spatial Data Formats:

Geospatial data comes in two primary types: [raster data](https://en.wikipedia.org/wiki/Raster_data) and [vector data](https://en.wikipedia.org/wiki/Vector_graphics).

You can read in greater detail about different geospatial data formats [on wikipedia](https://en.wikipedia.org/wiki/GIS_file_formats), but the basic data structures are:

- Raster data is data that is distributed on a grid.
    - A raster is just a grid of data, where each cell in the grid has some value (or values).
    - The cells are sometimes also called pixels. With image data, each pixel in the raster might have several values, such as the value of red, green and blue hues.
    - Image data thus has **bands**: each band is the information pertaining to the different colors.
- Vector data is data that is a collection of points or polygons distributed in space.
    - "Vector" in the geospatial context doesn’t mean the same thing as in a mathematial context: vector geospatial data includes any data that has vertices that can be anywhere in space.
    - This contrasts with raster data where the data points are fixed in space, usually on a rectangular grid.

These different kinds of data have different file formats. There is are two linked software libraries for processing these data, called [GDAL](http://www.gdal.org/) and [OGR](http://gdal.org/1.11/ogr/): they are commonly used in geospatial software so you should be able to convert between the data types that are read by these two packages. Click on the links for information:

- [Common raster file formats](http://www.gdal.org/formats_list.html).
- [Common vector file formats](http://www.gdal.org/ogr_formats.html).

[The Ultimate List of GIS Formats and Geospatial File Extensions](https://gisgeography.com/gis-formats/)

### GDAL

GDAL (the Geospatial Data Abstraction Library) is a popular software package for manipulating geospatial data. GDAL allows for manipulation of geospatial data in the Linux operating system, and for most operations is much faster than GUI-based GIS systems (e.g., ArcMap).

In the GDAL docs it recommends to use Conda to install GDAL however one of the alternatives that is not listed is [GIS Internals](https://www.gisinternals.com/) which provide a number of options including MSI installers for Windows for both the stable and development release.

For this workshop we will use stable version and this can be download using the instructions at GDAL site [https://gdal.org/download.html](https://gdal.org/download.html).

[https://github.com/rcc-uchicago/gdal_introduction](https://github.com/rcc-uchicago/gdal_introduction)

How do I get GDAL on my computer

Installation in Windows: 
1. Using network installer at OSGeo4W or  
2. using conda install -c  conda-forge gdal

Installation in Mac 
http://www.kyngchaos.com/software/frameworks/    echo 'export PATH=/Library/Frameworks/GDAL.framework/Programs:$PATH' >> ~/.bash_profile source ~/.bash_profile

Installation in Ubuntu: 
sudo apt-get install gdal-bin

### Basic Examples

One of the most frequent operations in GDAL is just to see what sort of data you have. The tool for doing this is gdalinfo which is run with the command line:

```bash
$ gdalinfo filename
```

where filename is the name of your raster. This is used mostly to:

See what projection your raster is in, and to check the extent of the raster.

```bash
$ gdalinfo col_viirs_100m_2016.tif
```

The output gives us a lot of information about this file including its coordinate system (if it has one), the pixel size, origin, metadata, the compression used on the file and the color ramp.

**OGRINFO:** open a new command prompt window in Data/Vector/Shapefile

```bash
ogrinfo gadm36_COL_1.shp
ogrinfo -so gadm36_COL_1.shp
ogrinfo -so gadm36_COL_1.shp gadm36_COL_1
```

### Geocomputation in R

In general, R requires dataset to be loaded into memory. A notable feature of the raster package is that it can work with raster datasets that are stored on disk and are too large to be loaded into memory(RAM). The package is built around a number of 'S4' classes of which the RasterLayer, RasterBrick, and RasterStack classes are the most important.
A RasterLayer object represents single-layer (variable) raster data. A RasterLayer object always stores a number of fundamental parameters that describe it. These include the number of columns and rows, the coordinates of its spatial extent ('bounding box'), and the coordinate reference system (the 'map projection').
In addition, a RasterLayer can store information about the file in which the raster cell values are stored (if there is such a file). A RasterLayer can also hold the raster cell values in memory.

The raster package has two classes for multi-layer data the RasterStack and the RasterBrick

**Open raster_basics_in_R.Rmd**

## Introduction to Slurm

• The RCC Midway compute systems

• Using Slurm (Simple Linux Utility for Resource Management) to submit jobs to the RCC Midway systems

Midway is a constellation a of many compute systems and storage with various architectures coupled together in one system.

Slurm is the software used to manage the workload on Midway.

![Geocomputation%20Workshop%207775d197cbc646d697b387c8e4d55b32/Untitled%201.png](Geocomputation%20Workshop%207775d197cbc646d697b387c8e4d55b32/Untitled%201.png)

Basic Definitions:

• A **processor** is a small chip that responds to and processes the basic instructions that drive a computer. The term *processor* is used interchangeably with the term **central processing unit** (CPU)

• **Core**: The smallest compute unit that can run a program

• **Socket:** A compute unit, packaged as one and usually made of a single chip often called processor. Modern sockets carry many cores (10, 14, or 20, 24, 28, etc. on most servers)

• **Node:** A stand-alone computer system that contains one or more sockets, memory, storage, etc. connected to other nodes via a fast network interconnect.

![Geocomputation%20Workshop%207775d197cbc646d697b387c8e4d55b32/Untitled%202.png](Geocomputation%20Workshop%207775d197cbc646d697b387c8e4d55b32/Untitled%202.png)

![Geocomputation%20Workshop%207775d197cbc646d697b387c8e4d55b32/Untitled%203.png](Geocomputation%20Workshop%207775d197cbc646d697b387c8e4d55b32/Untitled%203.png)

Schematic of the Midway Cluster

**Key Point to Remember**

There are about more than 1300+ nodes compute nodes on midway2, but only 2 login nodes.

This means you are sharing the login nodes with many other users at once. Running intensive programs on the login nodes causes the login nodes to be slow for all other users.

● login nodes are for editing files, compiling, moving files, changing permissions, and other non-intensive tasks.

● We recommend to use *sinteractive* for interactive runs

● For long running jobs => submit them to the queue

**Running Interactive jobs**

• login directly to a node

–Login to midway2.rcc.uchicago.edu

–Run the job at the command prompt

• Run interactively using sinteractive

– Uses Slurm to provide access to dedicated node(s) to which you can login directly

To use sinteractive:

```bash
sinteractive --time=01:00:00 --nodes=1
	--ntasks=2 --mem-per-cpu=2000 
	–-partition=broadwl
```

A **job** is the resources you are using and the code you are running

The **queue** in Slurm is all RUNNING and all PENDING jobs To see every job in the queue on Midway, use the command

```bash
squeue
```

to see jobs in the queue

```bash
squeue –u <cnetid>
or
myq
```

A batch script is list of instructions for slurm.

```bash
#!/bin/bash

# Here is a comment
#SBATCH --time=1:00:00

#SBATCH –nodes=1
#SBATCH –ntasks-per-node=1
#SBATCH --mem-per-cpu=2000
#SBATCH –job-name=MyJob
#SBATCH –output= MyJob-%j.out
#SBATCH –error=MyJob-%j.err

module load <module name>
#Run your code
```

This #! is a shebang. It tells operating system to use /bin/bash with this script. symbol # is a comment.  Everything after # is ignored by bash

**Running batch jobs**
using a Submission Script

A simple job submission script (saved as python.sbatch):

```bash
#!/bin/bash

#SBATCH --job-name=first_python_job
#SBATCH --output=first_python_job_%j.out
#SBATCH --error=first_python_job_%j.err
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --mem-per-cpu=2000M
#SBATCH --partition=broadwl
#SBATCH --time=00:30:00

module load python
python hello_world.py
echo “job finished at `date`”
```

To submit the above script:

```bash
sbatch python.sbatch
```

## Parallelization

> **Explicit parallelism** -- programmer must explicitly state which instructions can be executed in parallel, e.g. MPI

> **Implicit parallelism** -- automatic detection by compiler of instructions that can be performed in parallel. e.g. OpenMP

**OpenMP Job**

```bash
#!/bin/bash

#Here is a comment
#SBATCH --time=1:00:00
#SBATCH –partition=broadwl
#SBATCH –nodes=1
**#SBATCH –ntasks-per-node=8**
#SBATCH --mem-per-cpu=2000
#SBATCH –job-name=MyJob
#SBATCH –output= MyJob-%j.out
#SBATCH –error=MyJob-%j.err

module load <module name>
export OMP_NUM_THREADS=8 
#Run your code
./my_executable
```

OMP_NUM_THREADS is an environment variable.

**Parallel MPI job**

```bash
#!/bin/bash
#Here is a comment
#SBATCH --time=1:00:00
#SBATCH –job-name=MyJob
#SBATCH –output= MyJob-%j.out
#SBATCH –error=MyJob-%j.err
#SBATCH –partition=broadwl
#SBATCH –nodes=4
#SBATCH –ntasks-per-node=8
#SBATCH --mem-per-cpu=2000
module load openmpi
module load <module name>
#Run your code
mpirun  ./my_executable
```

## Writing simple Bash scripts for raster and vector processing

Arcpy 
```bash
export ARCGISHOME=$HOME/arcgis/server

# If you need only Python and arcpy you don't need to start the server.
# Note that you have to use ArcGIS' own Python installation instead of the
# default system installation. Python on ArcGIS Server for Linux runs
# a Windows version of Python under Wine.

# You start the ArcGIS Python console with:
/home/<user>/arcgis/server/tools/python

#If it does not work, Use Anaconda3 2019.03 module 
module load Anaconda3/2019.03
source activate arcpy

```

Open Python and test if 'import arcpy' works.

**Geocoding Example**

```bash
import arcpy
from arcpy import env
env.workspace = "home/pnsinha/Documents/Geocoding/atlanta.gdb
arcpy.CreateAddressLocator_geocoding("US Address - Dual Ranges", "streets Primary", "'Feature ID' FeatureID VISIBLE NONE;'*From Left' L_F_ADD VISIBLE NONE;'*To Left' L_T_ADD VISIBLE NONE;'*From Right' R_F_ADD VISIBLE NONE;'*To Right' R_T_ADD VISIBLE NONE;'Prefix Direction' PREFIX VISIBLE NONE;'Prefix Type' PRE_TYPE VISIBLE NONE;'*Street Name' NAME VISIBLE NONE;'Suffix Type' TYPE VISIBLE NONE;'Suffix Direction' SUFFIX VISIBLE NONE;'Left City or Place' CITYL VISIBLE NONE;'Right City or Place' CITYR VISIBLE NONE;'Left Zipcode' ZIPL VISIBLE NONE;'Right Zipcode' ZIPR VISIBLE NONE;'Left State' State_Abbr VISIBLE NONE;'Right State' State_Abbr VISIBLE NONE", Atlanta_AddressLocator, "", "DISABLED")

address_table = "customer"
address_locator =  "Atlanta_AddressLocator"
address_fields = "street Address; City City; State State; ZIP zip"
geocode_result = "geocode_result"
arcpy.GeocodeAddresses(address_table, address_locator, address_fields, geocode_result, 'STATIC')
```
GDAL commands

```
`gdalinfo lights.tif`

`gdal_merge –o mymerge.tif t27elu.dem t28elu.dem –ps 20 20`

Create contour lines

`gdal_contour mymerge.tif mycontours.shp –i 20`
Hillshade

`gdaldem hillshade mymerge.tif hillshade_30.tif –alt 45`
Slope

`gdaldem slope mymerge.tif slope.tif`

raster calculator

`gdal_calc --calc=“A*(A>80)” -A slope.tif --outfile=slopebin.tif`
```
### **More information**

[**OSGeo-Live](http://live.osgeo.org/en/index.html)** is a self-contained bootable DVD, USB thumb drive or Virtual Machine based on Lubuntu. We encourage using this Virtual Machine.

**XSEDE HPC Workshop Series**: [https://www.psc.edu/training-education/hpc-workshop-series](https://www.psc.edu/training-education/hpc-workshop-series)
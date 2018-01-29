# qnavigate
## QGIS plugin for navigation

### Work package #1: Load marine charts
* load marine charts [check free ones from OpenCPN: https://opencpn.org/OpenCPN/info/chartsource.html 
e.g. http://www.vnf.fr/ecdis/ecdis.html ; S57 is supported by GDAL]
  * **problem**: loading S57 seems to slow down QGIS horribly
  * **problem**: an appropriate styling should be created, following international guidelines; see whether something from OpenCPN can be recycled

### Work package #2: Load configurations
* choose polar for your boat; see sample in this repo
* define optional variables
  * % reduction for night navigation
  * % reduction for strong winds
  * avoidance of high waves
* define start and end points, plus optional intermediate points
* define start time

### Work package #3: Load meteo data
* download GRIB files (bounding box taken from points above; start date taken from above) [see http://www.zygrib.org/]
![Grib downolad popup](img/zygrib_download.png?raw=true "ZyGrib downolad popup")
  * possibly extract the bands of interest: ``gdal_translate -b 34 -b 35 -b 36 ECMWF0100_2017030100_000.grb wind.tif``
  * how to download from ECMWF: https://software.ecmwf.int/wiki/display/WEBAPI/Accessing+ECMWF+data+servers+in+batch
  * how to download from NASA: https://disc.gsfc.nasa.gov/information/howto/5761bc6a5ad5a18811681bae
  * proof of concept plugin: https://github.com/OpenDataHack/qgis-ecmwf-catalogue-plugin
  * **problem**: EPSG code / extent / bands possibly misinterpreted; see https://issues.qgis.org/issues/17219 and http://www.zygrib.org/forum/viewtopic.php?f=3&t=1069&p=3195#p3195
* configure an appropriate visualization (wind speed and direction, wave height and direction)
  * **problem**: find which bands convey the necessary info
  * **problem**: GRIB is loaded as a raster; not trivial to add wind direction and speed arrows, possibly a raster to vector conversion is necessary

### Work package #4: Routing
* calculate optimal route [see https://www.sailgrib.com/] and add as temporary layer
  * avoid land masses

### Work package #5: Navigation
* communicate with autopilot to set the helm
* protocol NMEA2000

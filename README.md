# qnavigate
## QGIS plugin for navigation

Draft specs:
* load marine charts [check free ones from OpenCPN: https://opencpn.org/OpenCPN/info/chartsource.html 
e.g. http://www.vnf.fr/ecdis/ecdis.html ; S57 is supported by GDAL]
  * **problem**: loading S57 seems to slow down QGIS horribly
  * **problem**: an appropriate styling should be created, following international guidelines; see whether something from OpenCPN can be recycled
* choose polar for your boat
* define optional variables
  * % reduction for night navigation
  * % reduction for strong winds
  * avoidance of high waves
* define start and end points, plus optional intermediate points
* define start time
* download GRIB files (bounding box taken from points above; start date taken from above) [see http://www.zygrib.org/]
![Grib downolad popup](img/zygrib_download.png?raw=true "ZyGrib downolad popup")
  * how to download from ECMWF: https://software.ecmwf.int/wiki/display/WEBAPI/Accessing+ECMWF+data+servers+in+batch
  * how to download from NASA: https://disc.gsfc.nasa.gov/information/howto/5761bc6a5ad5a18811681bae
  * proof of concept plugin: https://github.com/OpenDataHack/qgis-ecmwf-catalogue-plugin
  * **problem**: EPSG code / extent / bands possibly misinterpreted
* configure an appropriate visualization (wind speed and direction, wave height and direction)
  * **problem**: find which bands convey the necessary info
  * **problem**: GRIB is loaded as a raster; not trivial to add wind direction and speed arrows, possibly a raster to vector conversion is necessary
* calculate optimal route [see https://www.sailgrib.com/] and add as temporary layer
  * avoid land masses
* communicate with autopilot to set the helm

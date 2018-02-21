# qnavigate
## QGIS plugin for navigation

### Work package #1: Load marine charts
* load marine charts [check free ones from OpenCPN: https://opencpn.org/OpenCPN/info/chartsource.html 
e.g. http://www.vnf.fr/ecdis/ecdis.html ; S57 is supported by GDAL]
* create styles for them, see international guidelines: https://www.iho.int/iho_pubs/standard/S-52/PresLib_e3.4_Introduction.pdf and OpenCPN code here: https://github.com/OpenCPN/OpenCPN/blob/master/src/s52cnsy.cpp implementing "conditional symbology"; the code it tightly dependent on the OpenCPN internal data structures, so may be difficult to abstract directly. The basic idea is to walk the table of Features found in the S57 file(s), and draw each item in succession.  The S52 spec defines the order of rendering, so that the correct Features appear "on top".  There are also display categories, grouping Feature types together to control display content and complexity. etc...
  * **problem**: loading S57 seems to slow down QGIS horribly

### Work package #2: Load configurations
* choose polar for your boat; see sample in this repo
* define optional variables
  * % reduction for night navigation
  * % reduction for strong winds
  * avoidance of high waves
* define start and end points, plus optional intermediate points
* define start time

### Work package #3: Load meteo data (QMeteo)
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
  **QMeteo**
  * specific menu with a preloaded dataset of the world to allow users to make a direct bbox of the place of interest
  * extracting bands of meteorological interest and use styles to make weather forecasts (using also the autorefreshing system)

### Work package #4: Routing
* calculate optimal route
  * a description of possible approaches here: http://web.abo.fi/fak/tkf/at/ose/doc/Pres_15112013/Mikael%20Nyberg.pdf and https://www.researchgate.net/publication/280105634_Routing_and_course_control_of_an_autonomous_sailboat; on global planning level should be possible to use existing algorithms like A* or Dijkstra. But as in the sea there are no "roads" and we can move in any direction it is necessary to prepare data (nodes and edges) for these algorithms. Probabilistic
roadmap algorithms will work better as they don't need explicit edge-to-node connection. This would probably have to be implemented from scratch as a new QGIS alg. Also for the local planning stage seems a custom algorithm need to be developed
  * examples: [see https://www.sailgrib.com/], e.g. https://www.sailgrib.com/wp-content/uploads/2014/12/device-2015-09-01-125050_framed_tiny.jpg
* add the result as a temporary layer
  * avoid land masses from WP#1

### Work package #5: Navigation
* communicate with autopilot to set the helm
* protocol NMEA2000

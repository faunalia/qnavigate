# qnavigate
## QGIS plugin for meteo, dynamic routing, and navigation

Each work package is independent of others, and some (in particular `QMeteo` and `Dynamic routing`) are of very general interest.
The same approach can be useful in other context, e.g. paragliding (thanks Lene). Add more suggestions.

### Work package #1: QSea: load and style marine charts
*on standby - new standards are coming, better wait*
* load marine charts [check free ones from OpenCPN: https://opencpn.org/OpenCPN/info/chartsource.html 
e.g. http://www.vnf.fr/ecdis/ecdis.html ; S57 is supported by GDAL]
* create styles for them, see international guidelines: https://www.iho.int/iho_pubs/standard/S-52/PresLib_e3.4_Introduction.pdf and OpenCPN code here: https://github.com/OpenCPN/OpenCPN/blob/master/src/s52cnsy.cpp implementing "conditional symbology"; the code it tightly dependent on the OpenCPN internal data structures, so may be difficult to abstract directly. The basic idea is to walk the table of Features found in the S57 file(s), and draw each item in succession.  The S52 spec defines the order of rendering, so that the correct Features appear "on top".  There are also display categories, grouping Feature types together to control display content and complexity. etc...
  * **problem**: loading S57 seems to slow down QGIS horribly; possibly patches to GDAL from OpenCPN will solve this; merging back this work will require non trivial work; an alternative would be to convert in a GIS friendly format (GPKG?) at the beginning of the process

### Work package #2: Load navigation parameters
* choose polar for your boat; see [sample](./sunodyssey36i.pol) in this repo (more info at http://www.ockam.com/2013/06/03/what-are-polars/)
* define optional variables
  * % reduction for night navigation
  * % reduction for strong winds
  * avoidance of high waves
  * minimum sea depth
  * etc.
* define start and end points, plus optional intermediate points
* define start time

### Work package #3: QMeteo: load meteo data

*done*, see https://gitlab.com/faunalia/gribdownloader/ published in http://plugins.qgis.org/plugins/gribdownloader/

#### More issues related to QNavigate
* bounding box taken from points above; start date taken from above 
* also tides must be downloaded and taken into account for cosatal navigations; currents for open sea

#### further info on GRIB files
* http://openskiron.org/
* https://marine.meteoconsult.fr/cartes-meteo-marine/fichiers-grib.php
* see https://opengribs.org (previously http://www.zygrib.org/)
* notes here: https://nomads.ncdc.noaa.gov/data/gfsanl/IMPORTANT_NOTE 
* how to download from ECMWF: https://software.ecmwf.int/wiki/display/WEBAPI/Accessing+ECMWF+data+servers+in+batch
* how to download from NASA: https://disc.gsfc.nasa.gov/information/howto/5761bc6a5ad5a18811681bae
* proof of concept plugin: https://github.com/OpenDataHack/qgis-ecmwf-catalogue-plugin

### Work package #4: Dynamic routing
* calculate optimal route
  * a description of possible approaches here: http://web.abo.fi/fak/tkf/at/ose/doc/Pres_15112013/Mikael%20Nyberg.pdf and https://www.researchgate.net/publication/280105634_Routing_and_course_control_of_an_autonomous_sailboat; on global planning level should be possible to use existing algorithms like A* or Dijkstra. But as in the sea there are no "roads" and we can move in any direction it is necessary to prepare data (nodes and edges) for these algorithms. Probabilistic
roadmap algorithms will work better as they don't need explicit edge-to-node connection. This would probably have to be implemented from scratch as a new QGIS alg. Also for the local planning stage seems a custom algorithm need to be developed
  * examples: [see https://www.sailgrib.com/], e.g. https://www.sailgrib.com/wp-content/uploads/2014/12/device-2015-09-01-125050_framed_tiny.jpg
* add the result as a temporary layer
  * avoid land masses from WP#1
  * avoid other undesirable areas (e.g. crossing traffic corridors at right angles)

### Work package #5: Navigation
* communicate with autopilot to set the helm
* protocol NMEA2000 https://www.nmea.org/content/nmea_standards/nmea_2000_ed3_10.asp

### Work package #6: Porting to tablet
* QtQuick https://doc.qt.io/qt-5/qtquick-index.html
* Qfield http://www.qfield.org/

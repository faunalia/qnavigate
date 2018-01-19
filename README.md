# qnavigate
## QGIS plugin for navigation

Draft specs:
* load marine charts [check free ones from OpenCPN: https://opencpn.org/OpenCPN/info/chartsource.html 
e.g. http://www.vnf.fr/ecdis/ecdis.html ; S57 is supported by GDAL]
  * **problem**: loading S57 seems to slow down QGIS horribly
  * **problem**: an appropriate styling should be created, following international guidelines; see whether something from OpenCPN can be recycled
* choose polar for your boat; sample here:

``TWA\TWS	0	4	6	8	10	12	14	16	20	25	30	35	40	45	50	55	60
0	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0
5	0.0	0.3	0.4	0.5	0.7	0.7	0.8	0.8	0.8	0.8	0.7	0.5	0.1	0.0	0.0	0.0	0.0
10	0.0	0.5	0.8	1.1	1.3	1.5	1.5	1.6	1.6	1.5	1.4	1.0	0.2	0.1	0.1	0.0	0.0
15	0.0	0.8	1.2	1.6	2.0	2.2	2.3	2.4	2.4	2.3	2.1	1.5	0.5	0.2	0.2	0.0	0.0
20	0.0	0.9	1.4	1.8	2.3	2.5	2.7	2.7	2.8	2.7	2.4	1.8	0.8	0.4	0.2	0.0	0.0
25	0.0	1.1	1.7	2.2	2.7	3.0	3.2	3.2	3.3	3.2	2.9	2.1	1.2	0.5	0.2	0.0	0.0
32	0.0	1.8	2.8	3.6	4.5	5.0	5.3	5.4	5.5	5.3	4.8	3.5	2.6	1.1	0.4	0.0	0.0
36	0.0	2.2	3.3	4.2	5.0	5.6	5.9	6.0	6.1	6.0	5.6	4.9	4.0	1.6	0.5	0.0	0.0
40	0.0	2.6	3.7	4.6	5.5	6.1	6.3	6.4	6.5	6.4	6.0	5.4	4.8	1.9	0.8	0.0	0.0
45	0.0	3.0	4.2	5.2	6.1	6.6	6.8	6.9	7.0	6.9	6.5	6.0	5.7	2.1	0.9	0.0	0.0
52	0.0	3.4	4.8	5.8	6.6	7.1	7.3	7.4	7.5	7.4	7.1	6.5	6.4	2.3	1.0	0.0	0.0
60	0.0	3.8	5.1	6.1	6.9	7.3	7.5	7.6	7.7	7.7	7.3	6.8	6.8	2.7	1.4	0.0	0.0
70	0.0	4.0	5.3	6.4	7.0	7.4	7.6	7.8	8.0	8.0	7.7	7.2	7.2	2.9	1.4	0.0	0.0
80	0.0	4.0	5.4	6.5	7.2	7.5	7.7	8.0	8.3	8.3	8.0	7.4	7.4	3.0	1.5	0.0	0.0
90	0.0	4.0	5.4	6.5	7.2	7.6	7.8	8.0	8.5	8.5	8.2	7.7	7.7	3.5	1.9	0.4	0.4
100	0.0	4.0	5.4	6.6	7.3	7.6	7.9	8.2	8.7	8.7	8.4	7.9	7.9	4.0	2.4	0.4	0.4
110	0.0	4.0	5.4	6.6	7.3	7.7	8.1	8.5	8.9	9.1	9.0	8.6	8.6	4.7	3.0	0.9	0.9
120	0.0	3.8	5.2	6.4	7.1	7.6	7.9	8.4	9.3	9.7	9.7	9.5	9.5	5.7	3.8	1.0	1.0
130	0.0	3.4	4.8	6.0	6.9	7.4	7.7	8.1	9.0	9.7	10.0	9.9	9.9	6.4	4.5	1.5	1.5
140	0.0	3.0	4.4	5.5	6.5	7.1	7.5	7.9	8.7	9.7	10.5	10.7	10.7	8.0	5.9	2.1	1.6
150	0.0	2.6	3.9	5.0	5.9	6.8	7.3	7.6	8.4	9.8	11.2	12.0	12.0	9.6	7.2	2.4	2.4
160	0.0	2.3	3.5	4.5	5.4	6.4	7.0	7.3	8.1	9.2	10.7	12.2	12.2	10.4	7.9	3.1	2.4
170	0.0	2.1	3.2	4.1	5.0	6.0	6.7	7.1	7.8	8.7	10.0	11.2	11.2	10.6	8.4	3.4	2.8
180	0.0	1.9	2.9	3.9	4.7	5.7	6.4	6.9	7.5	8.3	9.3	10.2	10.2	10.2	8.2	3.1	2.6``

* define optional variables
  * % reduction for night navigation
  * % reduction for strong winds
  * avoidance of high waves
* define start and end points, plus optional intermediate points
* define start time
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
* calculate optimal route [see https://www.sailgrib.com/] and add as temporary layer
  * avoid land masses
* communicate with autopilot to set the helm

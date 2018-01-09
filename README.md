# qnavigate
QGIS plugin for navigation

Draft specs:
* choose polar for your boat
* define optional variables
  * % reduction for night navigation
  * % reduction for strong winds
  * avoidance of high waves
* define start and end points, plus optional intermediate points
* define start time
* download GRIB files (bounding box taken from points above; start date taken from above)
* calculate optimal route and add as temporary layer
* communicate with autopilot to set the helm

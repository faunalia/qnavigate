# qnavigate
## QGIS plugin for navigation

Draft specs:
* load marine charts
* choose polar for your boat
* define optional variables
  * % reduction for night navigation
  * % reduction for strong winds
  * avoidance of high waves
* define start and end points, plus optional intermediate points
* define start time
* download GRIB files (bounding box taken from points above; start date taken from above) [see http://www.zygrib.org/]
* configura an appropriate visualization (wind speed and direction, wave height and direction)
* calculate optimal route [see https://www.sailgrib.com/] and add as temporary layer
  * avoid land masses
* communicate with autopilot to set the helm

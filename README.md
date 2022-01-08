# utl-calculate-the-area-within-a-latittude-longitude-polygon
Calculate the area within a latittude longitude polygon 
    %let pgm=utl-calculate-the-area-within-a-latittude-longitude-polygon;

    Calculate the area within a latittude longitude polygon

    github
    https://tinyurl.com/ysdu34ph
    https://github.com/rogerjdeangelis/utl-calculate-the-area-within-a-latittude-longitude-polygon

    SAS Forum
    https://tinyurl.com/mrypuf8z
    https://communities.sas.com/t5/SAS-Programming/Estimate-area-using-geographic-coordinates-Lat-long/m-p/787617

    Stackoverflow
    https://tinyurl.com/mpeuzjak
    https://stackoverflow.com/questions/61975159/how-do-i-calculate-area-in-polygon-from-x-and-y-points-in-r

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */
    Problem Calucualte the area in nautical miles in a 1 degree square polygon around the equator


           Plot of 0.5° Longitude vs 0.5° Latitude
                  Arond the Equator

          -0.5°         0° LATITUDE             0.5°
         ---+------------------------------------+---
     LON |                    | 0°                  |
         |                    | L                   |
     0.5°+  + (-0.5°,0.5°)    | O    (0.5°,0.5°) +  + 0.5° NORTH
         |                    | N                   |
      0° |                    | G                   |
      L  |                    | I                   |
      O  |                    | T                   |
      N  |                    | U                   |
      G  |                    | D                   |
      I  |  0° EQUATOR        | E  0° EQUATOR        |
      T  |--------------------+---------------------+ 0.0° EQUATOR
      U  |                    | 0°                  |
      D  |                    | L                   |
      E  |  Area 3,600        | O                   |
         |  Square Nautical   | N                   |
         |  Miles             | G                   |
         |                    | I                   |
         |                    | T                   |
         |                    | U                   |
     0.5°+  + (-0.5°,-0.5°)   | D   (0.5°,-0.5°) +  + 0.5° SOUTH
         |                    | E                   |
         ---+------------------------------------+---
          -0.5°         0° LATITUDE             0.5°


    The earth is an elipsoid however for a back of an envelop check we will consider it a sphere.

    The radius of the earth is 3440 nautical miles, therefore one degree
    the distance between one degree of latitude at the equator is

    data _null_;
      diameter=2*constant('pi')*3440;
      one_degree_lat=diameter/360;
      putlog one_degree_lat=;
      *because we consider the earth as a shere one degree of longiture is also 60 nautical miles.;
      one_degree_long=one_degree_lat;
      putlog one_degree_long=;
    run;quit;

    ONE_DEGREE_LAT=60.039326269 nautical miles. ;
    ONE_DEGREE_LONG=60.039326269 nautical miles.;

    Therefore the area in square nautical mines at the equator is apprimately

         60.039326269**2 = 3604.7206988 square nautical miles
    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */
    For a more accurate estimate

    %let poly=c(-.5,-.5), c(.5,-.5), c(.5,.5), c(-.5,.5), c(-.5,-.5);

    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */

    %put &=area square nautical miles;

    AREA=3592.67731 square nautical miles

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */

    %let poly=c(-.5,-.5), c(.5,-.5), c(.5,.5), c(-.5,.5), c(-.5,-.5);

    %utl_submit_r64("
    library(tools);
    library(geosphere);
    onebyone <- rbind(&poly);
    area<-areaPolygon(onebyone)/(1851*1851);
    area<-format(area, nsmall=5);
    area;
    writeClipboard(area);
    ",returnvar=area);

    %put &=area square nautical miles;

    (1851*1851) converts square meters to nautical miles;

    * line plot;


    data pts;
    input lat lon;
    anno=cats('(',lat,'°,',lon,'°)');
    cards4;
    -.5 -.5
    .5 -.5
    .5 .5
    -.5 .5
    -.5 -.5
    ;;;;
    run;quit;

    options ls=64 ps=32;
    proc plot data=pts;
      plot lon*lat='+' $anno / box vref=0 ;
    run;quit;

    * Then edit with the powerful 1080's classical SAS editor

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */

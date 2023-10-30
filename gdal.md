# About the GDAL implementation of JSON-FG

At the time of testing, the JSON-FG driver is not yet part of a release but is merged into GDAL master. 

It can be for example tested using the -latest docker images mentioned at https://github.com/OSGeo/gdal/tree/master/docker

e.g

<pre>
$ docker run --rm -it ghcr.io/osgeo/gdal:alpine-small-latest ogrinfo --format jsonfg
Format Details:
  Short Name: JSONFG
  Long Name: OGC Features and Geometries JSON
  Supports: Vector
  Extension: json
  Help Topic: drivers/vector/jsonfg.html
</pre>

Or using Conda and the gdal-master channel. Instructions at https://gdal.org/download.html#gdal-master-conda-builds

We use the first method. 

Documentation of the GDAL JSON-FG driver is here: https://gdal.org/drivers/vector/jsonfg.html

GDAL implements the following features of JSON-FG: 
- capturing the feature type in a featureType element to affect features to separate
- capturing a coordinate reference system (not necessarily WGS 84) in a coordRefSys element, that is the one used by geometries written in the place element
- time element at Feature level
- minimum support for Polyhedron geometries (with a single outer shell) and Prism with Point, LineString or Polygon base.


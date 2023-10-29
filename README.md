# Test OGC Features and Geometries JSON
We use this repository to document the test scenarios and test results of the 

OGC Features and Geometries JSON - Part 1: Core DRAFT SPECIFICATION
https://docs.ogc.org/DRAFTS/21-045.html#_overview

and its implementation in GDAL
GDAL 3.8.0dev-d054405d9e063595400469128e142bd80bb71923, released 2023/09/16

as part of the OGC open standards code sprint, London, October 30 - November 1, 2023
https://developer.ogc.org/sprints/22/#:~:text=When%20is%20the%20code%20sprint,be%20held%20on%20October%2C%2012.

# About JSON-FG
OGC Features and Geometries JSON (JSON-FG) is a proposal for extending GeoJSON with support for a.o.

1. Temporal information
2. Additional geometry types
3. Coordinate reference systems (CRS) other than WGS84
4. Metadata about the feature types in a feature collection, using feature schemas

We test the JSON-FG draft specification and implementation on these components.

## Temporal information
JSON-FG adds to GeoJSON a standardized method to include temporal aspects for feature instances: date, time, and interval.

## Additional geometry types
GeoJSON only supports a limited number of geometry types in the geometry-attribute: (MULTI)POINT, (MULTI)LINE, (MULTI)POLYGON and GEOMETRYCOLLECTION. JSON-FG extends GeoJSON with 3D solid geometry to include (MULTI)POLYHEDRON and (MULTI)PRISM in a separate _place_ attribute.

## Coordinate reference systems
GeoJSON formally allows only geometry in the WGS'84 CRS to be included in the _geometry_ attribute. JSON-FG makes it possible to include geometry in other CRS in a separate _place_ attribute.

## Feature types and feature schema
JSON-FG describes how to add knowledge about the feature type in a feature collection, both homogeneous and heterogeneous in terms of the feature types, using for example feature schemas.


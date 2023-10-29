# About JSON-FG

OGC Features and Geometries JSON (JSON-FG) is a proposal for extending GeoJSON
with support for a.o.

1.  Temporal information

2.  Additional geometry types

3.  Coordinate reference systems (CRS) other than WGS84

4.  Metadata about the feature types in a feature collection, using feature
    schemas

We test the JSON-FG draft specification and implementation on these components.

## Temporal information

JSON-FG adds to GeoJSON a standardized method to include temporal aspects for
feature instances: date, time, and interval.

## Additional geometry types

GeoJSON only supports a limited number of geometry types in the
geometry-attribute: (MULTI)POINT, (MULTI)LINE, (MULTI)POLYGON and
GEOMETRYCOLLECTION. JSON-FG extends GeoJSON with 3D solid geometry to include
(MULTI)POLYHEDRON and (MULTI)PRISM in a separate *place* attribute.

## Coordinate reference systems

GeoJSON formally allows only geometry in the WGS'84 CRS to be included in the
*geometry* attribute. JSON-FG makes it possible to include geometry in other CRS
in a separate *place* attribute.

## Feature types and feature schema

JSON-FG describes how to add knowledge about the feature type in a feature
collection, both homogeneous and heterogeneous in terms of the feature types,
using for example feature schemas.

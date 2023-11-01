# Example datasets

During the OGC Code sprint we worked on JSON-FG example datasets. These datasets
touch on the various specific topics of JSON-FG, such as inclusion of geometry
with CRS other than WGS'84 in the place member, temporal information in the time
member and 3D geometries.

We created 6 JSON-FG example datasets:

1.  current topographic BGT features

2.  current and historic topographic BGT features

3.  Dutch municipalities from 2003

4.  Buildings as 3D Prims

5.  Buildings as Polyhedrals

6.  Circular arc interpolation

We used the GDAL draft implementation to convert from GPKG, PostgreSQL or GML
files to JSON-FG. All objects in the example datasets intersect with the
bounding box of the buffer of 200 meters around ‘Het Binnenhof’ in The Hague.

We will describe each dataset hereafter.

## current topographic BGT features

**Goal**

With this example file we want to demonstrate the inclusion of geometries in an
CRS other than WGS'84. The JSON-FG draft specification requires that geometries
with a different CRS must be included in the place member, and that geometries
that are valid GeoJSON geometry types in WGS’84 must be included in the geometry
member.

The specifications also allow to include both GeoJSON geometries in WGS'84 in
the geometry member, and the geometries in RD in place member. The advantage is
that the JSON-FG can also be read in a GeoJSON client. The disadvantage is
increasing the file size (and payload).

NOTE: A JSON-FG file can contain both geometry in WGS'84 in the geometry member
and geometry in RD in the place member.

**Process**

We retrieve a full download of the BGT including history from PDOK.

The complete BGT file from PDOK is filtered for the topographical objects
without an end time (eindregistratie IS NULL). The example datasets therefore
describes the current situation of the position in the LV-BGT on a reference
date around the end of October (last LV publication date:: lookup?).

All geometry types in the file, expressed in WGS'84, are also valid GeoJSON
geometries.

We create two example datasets with the GDAL draft implementation:

\- a dataset with only BGT features in RD in the place member

\- a dataset with geometry in WGS'84 in the geometry member and geometry in RD in
the place member

NOTE: With the GDAL layer creation option (lco) WRITE_GEOMETRY you can choose
wether you want geometries in both geometry and place member
(WRITE_GEOMETRY=YES) or in one of the two members (WRITE_GEOMETRY=NO).

**Result**

<pre>
{
    "type": "Feature",
    "featureType": "paal",
    "coordRefSys": "[EPSG:28992]",
    "properties": {
        "lokaalid": "G0518.1e0a6f158f7be183e053530a0b0a8a7d",
        "objectbegintijd": "2015-08-17T00:00:00Z",
        "tijdstipregistratie": "2015-08-17T14:26:33Z",
        "lv-publicatiedatum": "2018-06-21T16:17:38Z",
        "bgt-type": "niet-bgt",
        "plus-type": "afsluitpaal",
        "hectometeraanduiding": null
    },
    "geometry": {
        "type": "Point",
        "coordinates": [
            4.3123011,
            52.0814892
        ]
    },
    "place": {
        "type": "Point",
        "coordinates": [
            81320.4,
            455347.397
        ]
    },
    "time": null
}
</pre>

## 

## current and historic topographic BGT features

**Goal**

With this example file we want to demonstrate the use of temporal information in
JSON-FG datasets. The JSON-FG draft specification enables to include temporal
information in the time member of a file. The time member may contain an date
object, time object or interval object, or either all three of them. However,
there are requirements on including both a date and time object, thats is the
date in the time object must be equal to the date in the date object.All times
in JSON-FG

<details class="note">The are strict requirements on the use of multiple temporal objects in the
time member of an JSON-FG and all timestamps are in UTC.
</details>

**Process**

We use again the full download of the BGT including history from PDOK.

The complete BGT file from PDOK is filtered for the topographical objects, but
now we do not throw away the history.

The example datasets therefore describes both the historic situation and the
current situation of of the BGT features. All geometry types in the file,
expressed in WGS'84, are also valid GeoJSON geometries.

We create an example datasets with the GDAL draft implementation:

\- a dataset with tijdstipregistratie and eindregistratie in the interval object
of the time member.

<details class="note"> In GDAL you should cast the start time to the time_start and the end time
to an time_end, for example CAST(tijdstipregistratie as timestamp with time
zone) as time_start.
</details>

**Result**

<pre>
{
    "type": "Feature",
    "featureType": "vegetatieobject",
    "coordRefSys": "[EPSG:28992]",
    "properties": {
        "lokaalid": "G0518.1e0a6f1604dee183e053530a0b0a8a7d",
        "objectbegintijd": "2015-08-17",
        "objecteindtijd": "2019-06-26",
        "tijdstipregistratie": "2015-08-17T14:24:05",
        "eindregistratie": "2019-06-26T15:08:45",
        "lv-publicatiedatum": "2015-09-15T10:25:33",
        "bgt-type": "niet-bgt",
        "plus-type": "boom"
    },
    "geometry": null,
    "place": {
        "type": "Point",
        "coordinates": [
            81541.038,
            455189.515
        ]
    },
    "time": {
        "interval": [
            "2015-08-17T13:24:05Z",
            "2019-06-26T14:08:45Z"
        ]
    }
}
</pre>

## 

## Dutch municipalities from 2003

**Goal**

With this example file we again want to demonstrate the use of temporal
information in JSON-FG datasets.

**Process**

We download the ‘wijken-buurten-gemeenten’ datasets from 2003 to 2023 from the
CBS website, and put them in a PostGIS database. Next we combine all the files
according tot he municipality reorganizations since 2003:

1.  If a municipality appears in a file but not in the following year, the
    municipality has been dissolved.

2.  If a municipality does not appear in a file, but does appear in the
    following year, then a new municipality has been established.

We update the geometry with the latest available geometry in the datasets.

Ultimately we export to a GPKG-file with municipalities including the code,
name, creation date and termination date for each municipality.

**Remarks**

1.  municipalities that exist in the 2003 dataset and are not terminated, get
    creation date (‘2003-01-01’).

2.  when municipalities merge, a new municipality is legally created, but this
    has been ignored for simplicity reasons.

Finally, the GPKG file is exported to a JSON-FG using the GDAL draft
implementation.

**Result**

## Buildings as 3D Prisms

**Goal**

With this example file we want to demonstrate the use of 3D prisms in JSON-FG.
The JSON-FG draft specification describes an easy way to include extruded point,
line or polygon geometries.

**Process**

We use the LOD12_2d 3D BAG features as a source file. We import this layer in an
PostGIS-database and create a SQL-script that writes the GeoJSON-representation
of to the “base” property, the groundlevel to the “lower” property, and
70p-height to the “upper” property of the Prism.

**Result**

*To be extended…*

{

"type": "FeatureCollection",

"title": "SINGLE PRISM OBJECT FROM 3DBAG SURROUNDING BINNENHOF",

"conformsTo": ["[ogc-json-fg-1-0.1:core]"],

"features": [

{

"type": "Feature",

"properties": {"identificatie": "NL.IMBAG.Pand.0518100000203254"},

"geometry": {

"type": "Point",

"coordinates": [

4.310054697,

52.077976081

]

},

"place": {

"type": "Prism",

"base": {

"type": "Polygon",

"crs": {

"type": "name",

"properties": {"name": "EPSG:7415"}

},

"coordinates": [

[

[

81147.6875,

454955.46875

],

[

81155.7578125,

454946.59375

],

[

81162.0546875,

454951.375

],

[

81161.6171875,

454951.90625

],

[

81169.9453125,

454956.875

],

[

81168.2265625,

454960.5625

],

[

81165.03125,

454967.5625

],

[

81159.4609375,

454964.59375

],

[

81156.78125,

454962.5625

],

[

81147.6875,

454955.46875

]

]

]

},

"lower": 1.2849999666214,

"upper": 11.3428896665573

},

"time": {"date": "1934-01-01"}

}

]

}

## Buildings as Polyhedrals

*To validate…*

## Circular arc interpolation

**Goal**

With this example file we want to demonstrate the use of circular arc
interpolation in JSON-FG. Currently, circular arc interpolation in JSON-FG is
still under discussion and not part of the draft specification and . However, an
propsal for it is made in this document:
<https://github.com/opengeospatial/ogc-feat-geo-json/blob/main/proposals/circular-geometry-objects.adoc>

**Process**

The BGT test dataset contains features having CurvePolygon or CompoundCurves
geometries. However, the GDAL implementation of JSON-FG interpolates circular
arc strings to linestring segments, so we cannot use GDAL on our BGT dataset to
get an example feature of an CurvePolygon in JSON-FG.

As the proposal for circular arcs looks pretty much like JSONify of the WKT of
geometries, we alter the WKT geometry of the BGT to an JSON-FG-ish geometry in
the place member.

**Result**

*To be extended…*

{

"type": "Feature",

"featureType": "wegdeel",

"coordRefSys": "[EPSG:28992]",

"properties": {

"lokaalid": "G0518.1e0a6f175bbae183e053530a0b0a8a7d",

"objectbegintijd": "2015-08-17T00:00:00Z",

"objecteindtijd": null,

"tijdstipregistratie": "2018-06-21T14:23:20Z",

"eindregistratie": "2021-11-22T16:40:21Z",

"lv-publicatiedatum": "2018-06-21T16:17:38Z",

"bgt-fysiekvoorkomen": "open verharding",

"plus_fysiekvoorkomen": "gebakken klinkers",

"bgt-functie": "voetpad",

"plus-functie": null

},

"geometry": null,

"place": {

{

"type": "CurvePolygon",

"coordinates": [

{

"type": "CompoundCurve",

"coordinates": [

{

"type": "Arc",

"coordinates": [

[

81181.685,

455198.326

],

[

81180.715,

455196.105

],

[

81181.004,

455189.334

]

]

},

{

"type": "LineString",

"coordinates": [

[

81181.004,

455189.334

],

[

81186.059,

455181.659

],

[

81186.651,

455180.761

],

[

81191.142,

455173.585

],

[

81197.21,

455162.659

],

[

81199.115,

455158.495

],

[

81203.405,

455160.479

],

[

81195.976,

455176.612

],

[

81194.726,

455178.463

],

[

81194.436,

455179.056

],

[

81194.067,

455179.194

],

[

81194.132,

455179.411

],

[

81181.685,

455198.326

]

]

}

]

}

}

}

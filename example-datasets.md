Example datasets

During the OGC Code sprint we worked on JSON-FG example datasets. These datasets
touch on the various specific topics of JSON-FG, such as inclusion of geometry
with CRS other than WGS'84 in the place member, temporal information in the time
member and 3D geometries.

We created 5 JSON-FG example datasets:

1.  current topographic BGT features

2.  current and historic topographic BGT features

3.  Dutch municipalities from 2003

4.  Buildings as 3D Prims

5.  Buildings as Polyhedrals

We used the GDAL draft implementation to convert from GPKG, PostgreSQL or GML
files to JSON-FG. All objects in the example datasets intersect with the
bounding box of the buffer of 200 meters around ‘Het Binnenhof’ in The Hague.

We will describe each dataset hereafter.

**current topographic BGT features**

Goal

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

Process

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

Result

\<\<code\>\>

**current and historic topographic BGT features**

Goal

With this example file we want to demonstrate the use of temporal information in
JSON-FG datasets. The JSON-FG draft specification enables to include temporal
information in the time member of a file. The time member may contain an date
object, time object or interval object, or either all three of them. However,
there are requirements on including both a date and time object, thats is the
date in the time object must be equal to the date in the date object.All times
in JSON-FG

NOTE: The are strict requirements on the use of multiple temporal objects in the
time member of an JSON-FG and all timestamps are in UTC.

Process

We use again the full download of the BGT including history from PDOK.

The complete BGT file from PDOK is filtered for the topographical objects, but
now we do not throw away the history.

The example datasets therefore describes both the historic situation and the
current situation of of the BGT features. All geometry types in the file,
expressed in WGS'84, are also valid GeoJSON geometries.

We create an example datasets with the GDAL draft implementation:

\- a dataset with tijdstipregistratie and eindregistratie in the interval object
of the time member.

NOTE: In GDAL you should cast the start time to the time_start and the end time
to an time_end, for example CAST(tijdstipregistratie as timestamp with time
zone) as time_start.

Result:

\<\<code\>\>

"features": [

{

"type": "Feature",

"featureType": "wegdeel",

"coordRefSys": "[EPSG:28992]",

"properties": {

"lokaalid": "G0518.1e0a6f1751c9e183e053530a0b0a8a7d",

"objectbegintijd": "2015-08-17",

"objecteindtijd": "2016-02-13",

"tijdstipregistratie": "2015-08-17T14:14:31Z",

"eindregistratie": "2016-02-13T09:27:57Z",

"lv-publicatiedatum": "2015-09-15T10:25:33Z",

"bgt-fysiekvoorkomen": "gesloten verharding",

"plus_fysiekvoorkomen": null,

"bgt-functie": "rijbaan lokale weg",

"plus-functie": null

},

"geometry": {

"type": "Polygon",

"coordinates": [

[

…

]

]

},

"place": {

"type": "Polygon",

"coordinates": [

[

…

]

]

},

"time": {

"interval": [

"2015-08-17T14:14:31Z",

"2016-02-13T09:27:57Z"

]

}

},

**Dutch municipalities from 2003**

Goal:

With this example file we again want to demonstrate the use of temporal
information in JSON-FG datasets.

Process:

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

Remarks:

1.  municipalities that exist in the 2003 dataset and are not terminated, get
    creation date (‘2003-01-01’).

2.  when municipalities merge, a new municipality is legally created, but this
    has been ignored for simplicity reasons.

Finally, the GPKG file is exported to a JSON-FG using the GDAL draft
implementation.

Result:

Buildings as 3D Prims

With this example file we want to demonstrate the use of temporal information in
JSON-FG datasets. The JSON-FG draft specification enables to include temporal
information in the time member of a file. The time member may contain an date
object, time object or interval object, or either all three of them. However,
there are requirements on including both a date and time object, thats is the
date in the time object must be equal to the date in the date object.All times
in JSON-FG

Buildings as Polyhedrals

BGT current data

BGT temporal

Gemeenten

Prism

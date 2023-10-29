# TEST SCENARIOS

TODO: collect test cases, resultats etc in Excel spreadsheet.

For example:

## 

N.B. THIS IS AN EXAMPLE SPREADSHEET AND DOES NOT CONTAIN TRUE TEST RESULTS.

| \#ID    | Case description | Expectation                                                                 | Execution                  | Result | Remarks |
|---------|------------------|-----------------------------------------------------------------------------|----------------------------|--------|---------|
| GEOM001 | Prims            |                                                                             |                            |        |         |
|         | Polyhedron       |                                                                             |                            |        |         |
| CRS001  | Point in RD      | GDAL writes GML point geometry in RD to JSON-FG place attribute             | GDAL:: GML \> JSON-FG      | OK     |         |
|         |                  | GDAL writes JSON-FG place attribute to GML point geometry                   | GDAL:: JSON-FG \> GML      | OK     |         |
|         |                  | GDAL *tolerates* to write JSON-FG place attribute to GeoJSON point geometry | GDAL :: JSON-FG \> GeoJSON | NOT OK |         |
|         | Line in RD       |                                                                             |                            |        |         |
|         | Polygon          |                                                                             |                            |        |         |

## DRAFT SPECIFCATION

-   Geometry:

    -   (Multi)Point, (Multi)Line, Multi(Polygon), GeometryCollection in *place*
        attribute

    -   (Multi)Polyhedron and (Multi)Prism in *place* attribute

-   CRS:

    -   Types:

        -   WGS'84 (in geometry and place attribute)

        -   RD: 28992

        -   ETRS89: 4258

        -   3857?

        -   900913?

-   Temporal:

    -   CreationDate

    -   tijdstipregistratie

    -   Tijdlijn geldigheid vs. tijdlijn registratie

-   Feature metadata

    -   ?? Feature schema: JSON schema of BGT?

        -   Combine two datasets of BAG and BGT in 1 dataset?

## GDAL IMPLEMENTATION

-   Formats JSON-FG \<\>:

    -   GML

        -   GeoJSON

            -   PostgreSQL

            -   OpenFileGeodatabase (ESRI)

            -   Esri Shapefile

        -   CRS:

            -   Syntax/Format:

                -   URI: "http://www.opengis.net/def/crs/EPSG/0/28992"

                    SAFE CURIE: "[EPSG: 3857]"

        -   Others:

            -   Arcs?

            -   

## TEST EXECUTION

-   Datasets:

    -   BGT GML (2D RD, and includes ARCs)

        -   BAG 3D Geopackage (Polyhedrons)

        -   BAG extruded polygons (Prims)

        -   Bestuurlijke gebieden (temporal aspects)

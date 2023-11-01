# Considerations

## Circular arcs in JSON-FG
We support the [proposal](https://github.com/opengeospatial/ogc-feat-geo-json/blob/main/proposals/circular-geometry-objects.adoc) to add circular arcs to JSON-FG. Dutch base registry data from the large scale topography dataset (BGT) contains a lot of arcs. We created some small test files containing such arcs and it seems that this costs little effort to support, and satisfies a requirement that is not uncommon. 

## Tool support for JSON-FG
In order to support adoption and implementation of JSON-FG, it would be helpful to have more basic tooling that supports JSON-FG, such as:
- validators to test if a JSON-FG file conforms to the standard. Not all requirements are covered by the JSON Schema. We are working on a linter in [JSON-FG linter](https://github.com/Geonovum-labs/json-fg-linter), more validators would be useful. 
- tooling to read/write JSON-FG. We are working on a [JSON-FG encoding and decoding library for Java](https://github.com/Geonovum-labs/json-fg-java). More tooling like this would useful.
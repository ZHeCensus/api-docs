---
title: Mapbox choropleth map
description: This is a examples page.
permalink: /examples/mapbox-choropleth/

layout: post
---

This example shows how to setup a choropleth map using mapbox, to show the [GINI Index of all US Counties](https://www.census.gov/topics/income-poverty/income-inequality/about/metrics/gini-index.html) from ACS 5-year (2017). Mapbox vector titles are used as it provides better performance then leaflet when rendering large amount of data. If you would like to use GeoJSON check out the Additional Notes below, but it is discouraged to load all counties via GeoJSON due to the large size.

See completed example [here]({{ '/examples/live/mapbox-choropleth/' | relative_url }}){:target="\_blank"}

## Setting up

Import libraries and styles in the head. You can download citysdk.js [here]({{ '/assets/citysdk.js' | relative_url }}){:download="citysdk.js"} or build it using browserify.

```html
<script src="https://api.tiles.mapbox.com/mapbox-gl-js/v1.2.0/mapbox-gl.js"></script>
<link
  href="https://api.tiles.mapbox.com/mapbox-gl-js/v1.2.0/mapbox-gl.css"
  rel="stylesheet"
/>
<script src="./citysdk.js"></script>
```

In the body setup a container with an id of map, with a height and width.

```html
<style>
  body {
    margin: 0;
    padding: 0;
  }
  #map {
    position: absolute;
    top: 0;
    bottom: 0;
    width: 100%;
  }
</style>
<div id="map"></div>
```

In a script tag after the map div, we initialize the map.

```js
mapboxgl.accessToken = "<your access token here>";
var map = new mapboxgl.Map({
  container: "map",
  style: "mapbox://styles/mapbox/streets-v11",
  center: { lat: 37.0902, lng: -95.7129 },
  zoom: 3
});
```

## Adding the Vector tiles

Census Bureau provides vector tiles via [ArcGIS REST Services](https://gis-server.data.census.gov/arcgis/rest/services/Hosted){:target="\_blank"} for various years and geographic levels.

To locate the vector layer we want first find the year then the geography level id.

Here are a few geographic level ids for common levels:
010 = United States
020 = Region
030 = Division
040 = State
050 = ..... County
060 = ..... ..... County Subdivision
140 = ..... ..... Census Tract
860 = ..... 5-Digit ZIP Code Tabulation Area
Find more [here](https://factfinder.census.gov/service/GeographyIds.html){:target="\_blank"}

So for 2017 and county level we can look for
`2017` and `050` using find.

![Vector tile for 2017 counties (050)]({{ '/assets/images/examples/example-choropleth-mapbox1.png' | relative_url }})

The layer is [Hosted/VT_2017_050_00_PY_D1](https://gis-server.data.census.gov/arcgis/rest/services/Hosted/VT_2017_050_00_PY_D1/VectorTileServer). Lastly we have to get the source-layer name. Click into the Styles and find the source-layer.

![Vector tile's page, with style link underlined]({{ '/assets/images/examples/example-choropleth-mapbox2.png' | relative_url }})

!["source-layer" : "County"]({{ '/assets/images/examples/example-choropleth-mapbox3.png' | relative_url }})

Now lets load the vector layer.

```js
map.on("load", function() {
  map.addLayer({
    id: "counties",
    type: "fill",
    source: {
      type: "vector",
      tiles: [
        "https://gis-server.data.census.gov/arcgis/rest/services/Hosted/VT_2017_050_00_PY_D1/VectorTileServer/tile/{z}/{y}/{x}.pbf"
      ]
    },
    "source-layer": "County",
    paint: {
      "fill-opacity": 0.6,
      "fill-color": "blue"
    }
  });
});
```

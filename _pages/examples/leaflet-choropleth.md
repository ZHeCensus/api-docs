---
title: Choropleth map using leaflet
description: This is a examples page.
permalink: /examples/leaflet-choropleth/

layout: post
sidenav: examples
---

This example shows how to setup a choropleth map using leaflet to display a percentage. See completed example [here]({{ '/examples/live/leaflet-choropleth/' | relative_url }}). Adapted from [leaflet's example](https://leafletjs.com/examples/choropleth/)

<!-- <iframe src="{{ '/examples/live/leaflet-choropleth/' | relative_url }}"></iframe> -->

```html
<link
  rel="stylesheet"
  href="https://unpkg.com/leaflet@1.5.1/dist/leaflet.css"
/>
<script src="https://unpkg.com/leaflet@1.5.1/dist/leaflet.js"></script>
<script src="{{ '/assets/citysdk.js' | relative_url }}"></script>
```

In the body setup a container with an id of map, with a height and width.

```html
<style>
  #map {
    height: 600px;
    width: 600px;
  }
</style>
<div id="map"></div>
```

Init the map

```js
var map = L.map("map").setView([28.466944, -82.498148], 7);

L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
  attribution:
    '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
}).addTo(map);
```

Call CitySDK and add layer in callback

```js
census(
  {
    vintage: 2017,
    geoHierarchy: {
      state: {
        lat: 28.466944,
        lng: -82.498148
      },
      county: "*"
    },
    geoResolution: "5m",
    sourcePath: ["acs", "acs5", "subject"],
    // Total!!Estimate!!Total population : S0102_C01_001E
    // 60 years and over!!Estimate!!Total population : S0102_C02_001E
    values: ["S0102_C01_001E", "S0102_C02_001E"]
  },
  function(error, response) {
    L.geoJson(response).addTo(map);
  }
);
```

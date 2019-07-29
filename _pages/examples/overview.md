---
title: Examples
description: This is a examples page.
permalink: /examples/

layout: post
sidenav: examples
subnav:
  - text: Quick Reference Examples
    href: "#quick-reference-examples"
  - text: Saving the file locally
    href: "#saving-the-file-locally-in-nodejs-using-fs"
  - text: Getting all counties
    href: "#getting-all-counties"
---

### Leaflet

- Simple leaflet
- Leaflet choropleth map

### Mapbox

- Mapbox choropleth map
- Mapbox dymanic loading

## Quick Reference Examples

### Getting all counties

```js
census({
  vintage: "2017",
  geoHierarchy: {
    county: "*"
  },
  sourcePath: ["acs", "acs5"],
  values: ["B19083_001E"], // GINI index
  statsKey: "<your key here>",
  geoResolution: "500k"
});
```

### Saving the file locally in Node.js using [`fs`]

```js
var fs = require("fs");

census(
  {
    vintage: 2017,
    geoHierarchy: {
      "metropolitan statistical area/micropolitan statistical area": "*"
    },
    geoResolution: "500k" // required
  },
  (err, res) => {
    fs.writeFile("./directory/filename.json", JSON.stringify(res), () =>
      console.log("done")
    );
  }
);
```

[`fs`]: https://nodejs.org/api/fs.html

This would convert the returned geojson to a string, which allows it to be saved via Node.js' fileSystem API.

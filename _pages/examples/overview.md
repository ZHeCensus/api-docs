---
title: Examples
description: This is a examples page.
permalink: /examples/

layout: post
sidenav: examples
---

### Quick Reference Examples

#### Example: Saving the file locally in Node.js using [`fs`]

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

#### All Counties

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

In this example, we use `citysdk` to create the payload and then save it via Nodes [`fs.writeFileSync`] and then serve it via a [Mapbox-GL] map.

[`fs.writefilesync`]: https://nodejs.org/api/fs.html#fs_fs_writefilesync_file_data_options
[mapbox-gl]: https://www.mapbox.com/mapbox-gl-js/api/

[![counties](https://raw.githubusercontent.com/uscensusbureau/citysdk/gh-pages/examples/assets/images/counties.PNG)](https://uscensusbureau.github.io/citysdk/examples/mapbox/counties_static/index.html)

[source code](https://github.com/uscensusbureau/citysdk/tree/gh-pages/examples/mapbox/counties_static)

#### Dynamic Use Example

A more dynamic example of using stats merged with GeoJSON on the fly with `citysdk` can be found here:

[![mapbox-geocoding](https://raw.githubusercontent.com/uscensusbureau/citysdk/gh-pages/examples/assets/images/mapbox-geocoding.png)](https://uscensusbureau.github.io/citysdk/examples/mapbox/with-mapbox-gl_geocoding_hover/index.html)

Type in a county name and see the unweighted sample count of the population (ACS) for all the Block Groups within that County.

Use Chrome for best results (mapbox-gl geocoder caveat)

[source code](https://github.com/uscensusbureau/citysdk/tree/gh-pages/examples/mapbox/with-mapbox-gl_geocoding)

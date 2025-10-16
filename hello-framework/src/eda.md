---
title: EDA – RailMapper Project
---

# RailMapper Project

Welcome to Kevin Chen and Zhiyuan (Will) Shao's XN Project. We are exploring the disaster recovery trends of boston railways.
Let's start by seeing the raw datasets:

## Edge Table
```js
Inputs.table(await FileAttachment("/data/NEC_edgeTable_29Sep2025.csv").csv())
```

## Node Table
```js
Inputs.table(await FileAttachment("/data/NEC_nodeTable_29Sep2025.csv").csv())
```

## Integrity Timeseries - Each column represents 8.45 hours (1 means station is working)
```js
Inputs.table(await FileAttachment("/data//node_integrity_timeseries_01_29Sep2025.csv").csv())
```

## Exploring data:
```js
//Load node data
const nodes = (await FileAttachment("/data/NEC_nodeTable_29Sep2025.csv").csv())
  .map(d => ({ ...d, lat: +d.lat, lon: +d.lon }))
  .filter(d => !isNaN(d.lat) && !isNaN(d.lon));
```
```js
//Basic lat vs long scatterplot to visualize transit shape
Plot.plot({
  width: 600,
  height: 400,
  marks: [
    Plot.dot(
      (await FileAttachment("/data/NEC_nodeTable_29Sep2025.csv").csv())
        .map(d => ({ ...d, lat: +d.lat, lon: +d.lon }))
        .filter(d => !isNaN(d.lat) && !isNaN(d.lon)),
      {
        x: "lon",
        y: "lat",
        title: "Name",
        r: 3,
        fill: "steelblue"
      }
    )
  ],
  x: { label: "Longitude" },
  y: { label: "Latitude" },
  title: "Boston Railway Stations",
  style: "background: #f0f8ff;"
})
```

Even without a map of the Boston area, you can see the general shape of the MBTA system plotted using latitude & longitude values, and how it contours with the actual shape of the Boston area. 

## Reproducibility:
Data:

Files taken from project details PDF. Original data files were .xlsx, converted to .csv to work easier with Observable

Data location:

Stored in src/data/

Files must be manually placed in src/data/

Environment setup:

npm install
npm install @observablehq/plot

## Challenges/risks:
One challenge we arn into already was the files being in excel format, we played around with using xlsx library to try and parse them which was very difficult before we conclucded to just converting them to .csv.
Another challenge up to this point was finding a working map, we found a .osm file of the massachusetts area, but converting to .geoJSON with an online converter only showed a paywall. We will need to find a way around this to display the Boston map on the lat/long chart.
Depending on how we continue geographic mapping will require third party source usage which may change/become unavailable in the future which would break the visualization.
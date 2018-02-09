# Getting started

Install the NuGet package:
``` ps
Install-Package SpatialLite.Core
```

Use library:
``` c#
var geometry = WktReader.Parse<LineString>("linestring (-10.1 15.5, 20.2 -25.5, 30.3 35.5)");

Console.WriteLine("Start point: {0}", geometry.Start);
Console.WriteLine("End point: {0}", geometry.End);

// Writes:
// Start point: [-10.1; 15.5; NaN, NaN]
// End point: [30.3; 35.5; NaN, NaN]
```


Write a GPX:
``` c#
List<GpxPoint> pts_collection = new List<GpxPoint> { };
// Create a point
GpxPointMetadata meta = new GpxPointMetadata();
Coordinate coords = new Coordinate(-72.5, 46.5, 173);
GpxPoint pts = new GpxPoint(coords);
pts.Timestamp = DateTime.ParseExact("2018 01 01 05:42:00", "yyyy MM dd HH:mm:ss", System.Globalization.CultureInfo.InvariantCulture); 
meta.Pdop = 3.1;
meta.Source = "Sat"
pts.Metadata = meta;

// Add the point to the collection
pts_collection.Add(pts);

// Create the tracklog
GpxTrackSegment trkseg = new GpxTrackSegment(pts_collection);
List<GpxTrackSegment> trkseg_col = new List<GpxTrackSegment> { trkseg };
GpxTrack trk = new GpxTrack(trkseg_col);

// Write the GPX file
GpxWriterSettings gpxSettings = new GpxWriterSettings();
gpxSettings.GeneratorName = "SomeOne";
GpxWriter gpxWriter = new GpxWriter("C:\\gpxfile.gpx", gpxSettings);
gpxWriter.Write(trk);
gpxWriter.Dispose();

```

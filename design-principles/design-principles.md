# Architectural Design Principles 
## Background

**WOW initiative** : the key to data sharing is to agree an API structure, data format, encoding and shared vocabulary. this document describes the agreed principles on which we will base the development of Weather on the Web standards. 

# Architectural principles

The design of the APIs will based on the OGC&#39;s WFS3.0 specification, and support sufficiently rich metadata to support future discovery services.  The metadata will also describe the provenance of the data and its geospatial/temporal properties. There will be a &quot;family&quot; of APIs so reflecting the varying complexity of the supported use cases. This will range from a simple API for point extraction for example, to more advanced extraction patterns such as vertical profiles. These APIs are documented using the OpenAPI 3.0 specification.

1. The API is for data only. The implication of this is that the API does not contain or reference any &quot;business logic&quot;
2. The API will not reflect the design implementation used to support it, i.e anything sever side, so ensuing a complete de-coupling from the server.
3. The API will support all three main underlying data types (i.e. observations, imagery and multi-dimensional gridded data. Thus, all data will be considered by its geometry not type. Identifiers (e.g. WMO codes, ICAO blocks) may be used as a proxy for a geospatial location.
4. The URL consisting of the service instance plus &quot;/&quot; will return a &quot;landing&quot; page that provides a top-level description of the resource in html.
5. The API will support a number of query patterns, i.e

| **Feature class** | **Feature**** identifiers** |
| --- | --- |
| **Point Features** | Point | PointCollection | PointTimeSeries | PointCollectionTimeSeries |
| **Profile Features** | Profile | ProfileCollection | ProfileTimeSeries | ProfileCollection TimeSeries |
| **Section Features** | Section | SectionTime |   |   |
| **Grid Features** | Grid | GridLayer | GridLayerTimeSeries |   |
| **Polygon Features** | Polygon | PolygonCollection | PolygonTimeSeries |   |
| **Trajectory Features** | 2D Trajectory | 2D TrajectoryTime | 3D Trajectory | 3D TrajectoryTime |

1. The API will form a&quot; family&quot; through the principle of inheritance, using the Open API specification.
2. Any one implementation of the API does not have support all query patterns (&quot;aka feature types).
3. The API will use the OGC&#39;s WFS3.0 as a base and be conformant with it, if possible.
4. The API will support a full set of metadata that will not only describe the accessed data, but
  1. The query patterns that are appropriate to each feature.
  2. The Metadata will advertise to the &quot;client user&quot; the supported extraction types.
  3. All default values will be advertised and choices e.g. interpolation type, possible units etc. will be enumerated.
  4. A full description of the data provenance and geospatial/temporal properties

## Feature Data Demonstrator
The Met Office has developed a basic demonstrator to show how a common and consistent API format could be used to provide access to global point feature data. This includes: a map UI for point selection; and an intelligent geo-proxy which accepts lat long requests from the UI. 

The Geo Proxy maintains a registry of global country boundaries and links these to the most appropriate source of data for that location (if one exists). An admin screen enables selection of country boundaries, and mapping of URLs for the standardised source of data.

Currently the demonstrator adapts the different point feature APIs from UK Met Office, Met Norway and NOAA and presents these in the common format described by the proposed [Feature Data Format](https://github.com/opengeospatial/weather-on-the-web/blob/master/Specification/Feature%20Data%20Format%20proposal.md)
This is not ideal as a different adapter would be required for each participating data provider, the preferred approach is for each data center to provide an API that conforms to the standard.

The Demonstrator and Admin screen are available here:
[Feature Data Demonstrator](http://labs.metoffice.gov.uk/map/owdemo/)

[Feature Data Admin Screen](http://labs.metoffice.gov.uk/map/owdemo/admin.html)

Note: Using one of the existing URLs one can associate this with any country boundary:

`Select a country`

`paste URL: http://labs.metoffice.gov.uk/ow-concept-demo/query/v1/datapoint/forecast.json`

`Save Changes`

Use the demonstrator to select a point within the boundary of the updated country.

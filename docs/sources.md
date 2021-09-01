Data sources
============

[New York State Geospatial Data Clearinghouse](http://gis.ny.gov/) \
Streets, Civil Boundaries.  Has a "statewide" layer of parcels, but it does not include Niagara nor 35 other counties

[OpenStreetMap data extracts (Geofabrik)](https://download.geofabrik.de/) \
Downloads by state/province.  Use the .shp.zip downloads.

[CUGIR (Cornell University Geospatial Information Repository)](https://cugir.library.cornell.edu/) \
Microsoft buildings for NY counties

[Canadian Building Footprints (Microsoft)](https://github.com/Microsoft/CanadianBuildingFootprints) \
Available by province.  Warning: Ontario is 808 MB, in GeoJSON format, and would take some time to process and convert.

Niagara Falls, Ontario -- [Webmap](https://niagarafalls.ca/city-hall/information-systems/gis/interactive-mapping.aspx) -- [Data](https://niagarafalls.ca/services/open/data)

Niagara County, NY -- [Webmap](https://niagara-county.maps.arcgis.com/apps/webappviewer/index.html?id=b5be67cf0e05477e8f4ad3161ab51422) \
No data website, but we can load the data from the underlying ArcGIS REST Server and save it to a local file.

In QGIS:
  1. Zoom to your area of interest
  2. Layer menu > Data Source Manager > ArcGIS REST Server
  3. Click "New" button to create a new connection.  Set Name = "Niagara County", and paste this URL:
      https://gis.niagaracounty.com/arcgis/rest/services/NC_GIS/NC_GIS/FeatureServer/
  4. Click "OK" to save the new connection
  5. Make sure "Niagara County" is selected, and click "Connect"
  6. Select "Parcel" (or whatever other layer you want)
  7. Check the box "Only request features overlapping the current view extent"
  8. Click "Add" and wait for the data to load (it may take a while depending on the number of features)

Once it has finished loading, right-click the layer > Export > Save Features As...
  1. Save as format: GeoPackage
  2. Don't just type a filename, or who knows where it will go!  Always click the "..."


Searching for GIS data
======================

Many other datasets can be found online.  Geolode is a searchable catalog of websites that offer free, open geospatial data:
* <https://geolode.org/>

If you don't find what you need there, try searching the web for combinations like:
* niagara gis data
* niagara shapefile
* niagara shp

Feel free to contact me at kgj2@cornell.edu for help finding geospatial data, or set up an appointment at <https://guides.library.cornell.edu/gis/help>

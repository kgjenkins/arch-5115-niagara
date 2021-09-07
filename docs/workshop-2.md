# ARCH 5115 - QGIS Workshop, Day 2

Workshop 2021-09-01 by Keith Jenkins, GIS Librarian at Mann Library

For help after this workshop, contact me at kgj2@cornell.edu  
Or set up a Zoom appointment at <https://guides.library.cornell.edu/gis/help>

All the data and documentation for this workshop can be downloaded from:  
<https://github.com/kgjenkins/arch-5115-niagara/archive/main.zip>

* Unzip the files


## Elevation raster data

Various scales of elevation data is available for the Niagara Falls region.  The National Elevation Dataset (NED) covers most of the US at resolutions of 1/3 arc-seconds (~10-meter pixels), and similar elevation data is available for New York State as a 10-meter DEM (digital elevation model) from the USGS/NYSDEC.  Sometimes there are local LiDAR surveys that can provide even finer detail, but unfortunately, the 1-meter and 2-meter elevation datasets only cover a small portion of the Niagara Falls area.  All of these datasets can be previewed and downloaded from:
<https://orthos.dhses.ny.gov/>

Due to the large size of these elevation datasets, downloads are divided into smaller tiles.  Tiles covering portions of Niagara Falls have already been downloaded and unzipped for you.  If you need other areas, those tiles can be downloaded from the site above.

* Select all the DEM files in the "data/elevation/10m" folder and drag them onto your QGIS map.

Notice that each tile has a slightly different min/max range, which causes abrupt changes along adjacent tile edges.  To combine all the tiles into a single, seamless layer, we will create a virtual raster:

* Raster menu > Miscellaneous > Build Virtual Raster...
* For "Input layers", click the "..." button, select all the DEMs (names end with elu), then click "OK"
* Under "Virtual", click the "..." button, and select "Save to file"
* Save it as "elevation.vrt" in the same folder where the .dem files are located
* Click "Run"

Now you should have a seamless mosaic of the elevation data.  You can now remove the original files from your project, but don't delete them from your folder! (because the .vrt points out to those files)

* In the QGIS Layers panel, select all the original DEMs, then right-click > Remove Layer...

By default, raster data is displayed with a gray gradient, with higher values brighter.  We can change the style to bring out more details in the data.

* In the Layer Styling panel, change from "Singleband gray" to "Singleband pseudocolor"
* Try changing the color ramp to "Turbo"


## Value Tool plugin

Although we could use the "Identify" tool to click pixels and see their values, the "Value Tool" plugin will allow you to instantly show the values from raster layers without even having to click.

* Plugins > Manage and Install Plugins...
* Search all plugins for "value tool" and install it

The Value Tool will appear on your toolbar as a green circle with white "i".

* Click the Value Tool button to open the panel
* Check the box to enable the tool
* Move your cursor across the map to view the raster values

Try checking the elevations above and below the falls.  Are these values in feet or meters?
(Hint: check the [Wikipedia article for Niagara Falls, New York](https://en.wikipedia.org/wiki/Niagara_Falls,_New_York))


## Elevation hillshade

Even though we can see structure of the levees and streets with the color ramp, it still looks rather flat.  Let's use a hillshade to made it look more 3-dimensional.

* In the Layers Panel, right-click the elevation layer > Duplicate
* Right-click "elevation copy" > Rename to "hillshade"
* Move the "hillshade" layer just above "elevation" and check the box to display the hillshade layer
* Open the styling panel be sure that you are styling the "hillshade" layer
* Change "Singleband pseudocolor" to "Hillshade"
* Set the blending mode to "multiply" (which will let us also see the colorized version underneath)
* You can adjust the strength of the shadow by adjusting the Z factor
* Try turning the dial to adjust the angle of the "sun"


## 1-meter elevation data

A virtual raster has already been created for the 1m elevation data.

* Drag the "data/elevation/1m/elevation-1m.vrt" file onto QGIS

Try viewing it as a hillshade.  Use the value tool to compare elevations from the 1m and 10m elevation layers.


## Creating contour lines

There is a "contours" display style for rasters, but is just a visual effect.  If we want to export 3D contour lines for use in a program like Rhino or AutoCAD, we will need to create a vector contour layer.

* Open the processing toolbox (gear icon, or in the Processing menu)
* Search for "contour" and open the "Contour" tool under GDAL > Raster extraction
* Set the following parameters:
  * Input layer = "elevation-1m"
  * Interval between contour lines = 2m (this value should usually not be any less than the pixel size)
  * Check the box to "Produce 3D vector"
* Keep the other options and click "Run"

To export the 3D contours to DXF, right-click the layer > Export > Save features as ... AutoCAD DXF or else export your whole project to DXF.


## Imagery for specific years

While the Google and Big satellite basemaps are easy to use, we can't tell what year the imagery is from.  Most states have some sort of imagery program in which various portions of the state are photographed each year, eventually resulting in a time series of imagery.  Generally speaking, for most states, this started happening in the 1990s or early 2000s.  Although there are older UDSA aerial photographs dating back to the 1930s, those images are often not readily available online, since significant work must be done to digitize, georeference, mosaic, and serve the images.

For New York, the following URL points to a web service for a number of imagery sets for different years:
* https://orthos.its.ny.gov/arcgis/rest/services/wms/

If you open that link in a web browser, you'll see a list of the available layers.  We can add any of those layers to our map by connecting QGIS to that URL.

* Layer > Data Source Manager
* Select the "ArcGIS Map Service" tab (near the bottom of the list on the left)
* Click "New"
* Set Name = "NYS Orthoimagery"
* Paste the URL and click OK

That sets up the connection to the web service, which will now appear in the dropdown menu of available services.

* Click "Connect" and wait for the response (it may take several seconds)
* Ignore the years at the end of each layer, and look for the 2008 imagery
* Click the triangle to expand that item, and again
* Once you see a sub-item called "Image", select it
* Click the "Add" button (at the bottom of the dialog)

The imagery should appear on your map.  Note that various years only cover certain portions of the state.  (Niagara county was flown in 2002, 2005, 2008, 2011, 2014, and 2017.)

Any layers with "cir" (like "2005 cir") in the name is color-infrared, so vegetation will appear red (which makes it easier to distinguish plants from water, structures, and pavement).  Notice that the name of the layer is simply "Image", which isn't very helpful, so you may want to rename it now while you still remember what it represents.

* Right-click the Image layer > Rename to "Imagery 2008"

Imagery for other years (or any ArcGIS MapServer layer) can be added the same way.  Note that it is sometimes helpful to change your map CRS to match that of the ArcGIS MapServer layer -- right-click the layer name > Set CRS > Set Project CRS from Layer...


## Extracting data from a web map

Many counties and municipalities have webmaps available for the general public.  These maps let you view data like parcels and other layers in a web browser, but often don't provide an easy way to downlooad the underlying data.  However, it is sometimes possible to determine the web services that are being used behind the scenes, and then connect to those services from QGIS in order to get the raw data.

The [Nigara County Mapping website](https://www.arcgis.com/apps/webappviewer/index.html?id=b5be67cf0e05477e8f4ad3161ab51422) is a good example.

* Open that link in Google Chrome
* Zoom in to the city of Niagara Falls
* Turn on the "Parcels" layer (zoom in further if you don't see it appear)
* Press Ctrl-Shift-I to open the browser's developer tools
* Expand the panel to about half your screen
* Click the "network" tab at the top
* Move the map slightly to make it fetch more parcels
* Scroll down to the bottom of the network requests and copy the link URL of one of the "query?f=json..." requests

This URL is a web service that is used to provide parcel and other data to the web map.

* In QGIS, zoom to the city of Niagara Falls, so that the area you want is visible on the map
* Open the Data Source Manager (Layer menu)
* Click "ArcGIS REST Server" on the left
* At the top, click "New" and enter the following:
  * Name = Niagara County
  * URL = <https://gis.niagaracounty.com/arcgis/rest/services/NC_GIS/NC_GIS/FeatureServer/>
* Click OK to save the connection
* Make sure "Niagara County" is selected and click "Connect"
* Select the "Parcel" layer
* Check the box "Only request features overlapping the current view extent"
* Click "Add" and wait for the parcels to load

Once they have completely loaded, you can right-click the layer and export to a local geopackage or shapefile.

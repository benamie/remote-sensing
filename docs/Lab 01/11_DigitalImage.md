# Digital image

A digital image, sometimes called a [raster](https://en.wikipedia.org/wiki/Raster_graphics), is a block of pixels. Each pixel has a position, measured with respect to the axes of some coordinate reference system (CRS), a [geographic coordinate system](https://en.wikipedia.org/wiki/Geographic_coordinate_system) for example. A CRS in Earth Engine is often referred to as a projection, since it combines a shape of the Earth with a [datum](https://en.wikipedia.org/wiki/Geodetic_datum) and a transformation from that spherical shape to a flat map, called a [projection](https://en.wikipedia.org/wiki/Map_projection).

**[create image using colored squares showing how raster is put together. Two spatial scales. Smaller scale can point out individual pixel and larger can point out CRS]**

 

**![A picture containing diagram  Description automatically generated](/Users/ozzycampos/OneDrive/03_Projects/01_RemoteSensing/05_GoogleEarthEngine/Lab 01/clip_image002.png)**

 

Let’s view a digital image in GEE to better understand this concept:

1. In the map window of GEE, click on the Point geometry tool using the [geometry drawing tools](https://developers.google.com/earth-engine/playground#geometry-tools) to define your area of interest. As a reminder, you can find more information on geometry drawing tools in GEE’s Guides. Name the import point.

2. Import NAIP imagery by searching for 'naip' and choosing the *'NAIP: National Agriculture Imagery Program'* raster dataset. Name the import naip. 

3. Get a single, recent NAIP image over your study area and inspect it:

```javascript
//  Get a single NAIP image over the area of interest.  
var  image = ee.Image(naip  
                      .filterBounds(point)
                      .sort('system:time_start', false)
                      .first());      
//  Print the image to the console.  
print('Inspect the image object:', image);     
//  Display the image with the default visualization.  
Map.centerObject(point, 18);  
Map.addLayer(image, {}, 'Original image');
```

4. Expand the image object that is printed to the console. Expand the bands property and one of the bands (0, for example). Note that the CRS transform is stored in the crs_transform property and the CRS is stored in the crs property, which references an EPSG code. The CRS of our image is [EPSG:26910](http://spatialreference.org/ref/epsg/nad83-utm-zone-10n/). Often, you can learn more about those [EPSG codes](http://www.epsg-registry.org/) from [thespatialreference.org site](http://spatialreference.org/). The CRS transform is a list in the notation of [this reference](http://docs.oracle.com/javase/7/docs/api/java/awt/geom/AffineTransform.html).
   * [m00, m01, m02, m10 m11 m12] 
5. Access these data programmatically with the .projection() method:

```javascript
// Display the projection of band 0
print('Inspect the projection of band 0:', image.select(0).projection());
```

1. Note that the projection can differ by band, which is why it's good practice to inspect the projection of individual image bands. (If you call [.projection() on an image for which the projection differs by band, you'll get an error.) Explore the ee.Projection docs to learn about useful methods offered by the Projection     object. (To play with projections offline, try [this tool](http://www.giss.nasa.gov/tools/gprojector/).)
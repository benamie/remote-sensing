# Remote Sensing Background

## Overview

The purpose of this lab is to introduce digital images, datum, and projections, as well as demonstrate concepts of spatial, spectral, temporal and radiometric resolution. You will be introduced to image data from several sensors aboard various platforms. At the completion of the lab, you will be able to understand the difference between remotely sensed datasets based on sensor characteristics and how to choose an appropriate dataset based on these concepts. 

## Learning Outcomes

- Solid understanding of the following terms
  - Digital image
  - Datum resolution
  - Projection resolution
  - Spatial resolution
  - Spectral resolution
  - Temporal resolution
  - Radiometric resolution
- Navigate the Google Earth Engine console 
- Extract the attributes of a digital image
- Evaluate the projection of a digital image
- Visualize a digital image


# Section 1: Digital image

A digital image, sometimes called a [raster](https://en.wikipedia.org/wiki/Raster_graphics), is a block of pixels. Each pixel has a position, measured with respect to the axes of some coordinate reference system (CRS), a [geographic coordinate system](https://en.wikipedia.org/wiki/Geographic_coordinate_system), for example. A CRS in Earth Engine is often referred to as a projection, since it combines a shape of the Earth with a [datum](https://en.wikipedia.org/wiki/Geodetic_datum) and a transformation from that spherical shape to a flat map, called a [projection](https://en.wikipedia.org/wiki/Map_projection). 

**![A picture containing colored squares showing how raster is put together at two spatial scales: the smaller scale points out individual pixel and larger can point out CRS.](/Users/ozzycampos/OneDrive/03_Projects/01_RemoteSensing/05_GoogleEarthEngine/Lab 01/clip_image002.png)**

 
Let’s view a digital image in GEE to better understand this concept:

1. In the map window of GEE, click on the Point geometry tool using the [geometry drawing tools](https://developers.google.com/earth-engine/playground#geometry-tools) to define your area of interest. As a reminder, you can find more information on geometry drawing tools in GEE’s Guides. Name the import `point`.

2. Import NAIP imagery by searching for 'naip' and choosing the *'NAIP: National Agriculture Imagery Program'* raster dataset. Name the import `naip`. 

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

4. Expand the image object that is printed to the console by clicking on the dropdown triangles. Expand the property called `bands` and expand one of the bands (0, for example). Note that the CRS transform is stored in the `crs_transform` property underneath the band dropdown and the CRS is stored in the `crs` property, which references an EPSG code. 
** EPSG Codes** EPSG codes are 4-5 digit numbers that represent CRS definitions, of which each code represents a particular definition. The acronym EPGS, comes from the (now defunct) European Petroleum Survey Group.  The CRS of our image is [EPSG:26910](http://spatialreference.org/ref/epsg/nad83-utm-zone-10n/). You can learn more about those [EPSG codes](http://www.epsg-registry.org/) from [thespatialreference.org site](http://spatialreference.org/). The CRS transform is a list in the notation of [this reference](http://docs.oracle.com/javase/7/docs/api/java/awt/geom/AffineTransform.html).
   * [m00, m01, m02, m10 m11 m12] 
   
5. In addition to using the dropdowns, you can also access these data programmatically with the .projection() method:

```javascript
// Display the projection of band 0
print('Inspect the projection of band 0:', image.select(0).projection());
```

6. Note that the projection can differ by band, which is why it's good practice to inspect the projection of individual image bands. (If you call .projection() on an image for which the projection differs by band, you'll get an error.) Explore the ee.Projection docs to learn about useful methods offered by the Projection object. (To play with projections offline, try [this tool](http://www.giss.nasa.gov/tools/gprojector/).)

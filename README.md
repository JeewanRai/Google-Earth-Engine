# Google Earth Engine(Tutorial)
# Tutorial 2
[Click Here](https://www.geospatialecology.com/intro_rs_lab2/) for google like of the lession                                                        

![alt text](image.png)

![alt text](image-1.png)

![alt text](image-2.png)

```js
//Navigate to area of interest
Map.setCenter(132.5685, -12.6312, 8);
// Select a specific Sentinel-2 image from the archive
// Print image details to the Console
var sent2 = ee.Image("COPERNICUS/S2/20180422T012719_20180422T012714_T52LHM");
// Print image details to the Console
print(sent2)

// Add RGB composite to map, without parameters defined, before adding min and max value
Map.addLayer(sent2,{bands:['B4','B3','B2']}, "Black Layer", false);

//Map.addLayer(image, { bands: ['B4', 'B3', 'B2'],min: 0,max: 0.3,palette: ['red', 'green', 'blue'],
// opacity: 0.8,name: 'Landsat 8 RGB'}); without adding max and min value the region appears black
//Reflectance values for Sentinel-2 products range from 0 to 3000
// a true color composite typically refers to an image that is displayed using the red, green, and 
// blue bands of the satellite sensor, corresponding to the way human eyes perceive color.
Map.addLayer(sent2, 
{
  bands: ['B4', 'B3', 'B2'],
  min: 0,
  max: 3000,
  // cannot apply palette when there is multiple bands
}, 'Sentinel-2 RGB True Color Composite', false);
```
Output:                                                            
Black Layer                          
![alt text](image-3.png)                                                         
                                                                       True Color Composite                                   
![alt text](image-4.png)

In a false color composite, near-infrared, shortwave infrared, and other non-visible wavelengths are mapped to visible colors (red, green, and blue) to enhance the visualization of certain features. For example, vegetation, water bodies, and urban areas can be more easily distinguished using false color composites.                                          

![alt text](image-5.png)                                            

False color composites are essential in remote sensing and Earth observation because they allow us to visualize and analyze features and phenomena that are not easily detectable in natural color (true color) images.                                                                                                                                        

![alt text](image-7.png)                                                                                                                                        

![alt text](image-8.png)                                                                   
```js
Map.addLayer(sent2, 
{
  bands: ['B8', 'B4', 'B3'],
  min: 0,
  max: 3000,
  // cannot apply palette when there is multiple bands
}, 'Sentinel-2 RGB False Color Composite', false); // false will uncheck the layers so user can add layer as per requirement
```
Output:                                                             
False color composite                                                           

![alt text](image-6.png)                                            

To really build your understanding of how different wavelengths interact with surfaces, we are now going to load individual bands sequentially, from shorter to longer wavelengths. To display Band 1, the code is as follows:                                              

```js
Map.addLayer(sent2,{bands:['B1'], min:0, max:3000}, "Band B1", false);
Map.addLayer(sent2,{bands:['B4'], min:0, max:3000}, "B4", false);
Map.addLayer(sent2,{bands:['B8'], min:0, max:3000}, "B8", false);
Map.addLayer(sent2,{bands:['B12'], min:0, max:3000}, "B12", false);
```                                                                 
Output:                                                             
Band B1                                                                  

![alt text](image-9.png)                                          
 Band B4                                                                  
 ![alt text](image-10.png)                                              
 Band B8                                            
![alt text](image-11.png)                                           
Band B12                                                                
![alt text](image-12.png)


# Tutorial Lab 3
[[Click Here]](https://www.geospatialecology.com/intro_rs_lab3/) for google like of the lession                                                                 
Understanding spectral indices, each index is designed to highlight specific features on the Earth’s surface, such as vegetation health, water content, or soil properties.
For example, the Normalized Difference Vegetation Index (NDVI) measures the greenness of vegetation by comparing the reflectance of visible and near-infrared 
![alt text](image-13.png)
```js
//It is important to understand that we have now added access to the full Sentinel-2 image collection 
// (i.e. every image that has been collected to date) to our script, but we want only signgle cloude 
// free image from all the images; filter the image

var image = ee.Image(sent2 // defines variable to store result of following operations
.filterDate("2015-07-01", "2017-09-30") //sent2 referes to Sentinel-2 image collection object
.filterBounds(campus)
.sort("CLOUD_COVERAGE_ASSESSMENT")
.first());
```
Sentinel-2 data, CLOUD_COVERAGE_ASSESSMENT is not a function but a metadata property associated with the images. It provides a numerical value representing the percentage of the image that is covered by clouds. This property is used to assess the quality of the image in terms of cloud cover.

![alt text](image-14.png)
![alt text](image-15.png)                                      

```js
var imageCollection = ee.ImageCollection('COPERNICUS/S2')
  .filterDate('2020-01-01', '2020-12-31')  // Filter by date range
  .filterBounds(campus)  // Filter by geographic region
  .filterMetadata('CLOUDY_PIXEL_PERCENTAGE', 'less_than', 10)  // Filter by cloud coverage
  .sort('SUN_ELEVATION', false)  // Sort by sun elevation
  .first();  // Select the first image in the sorted collection

print(imageCollection);

```
Metadata is like the “extra information” that comes along with any piece of data. It doesn’t contain the actual content, but it provides context and details about that content.                

Wherever you click on the image, the band values at that point will be displayed in the Inspector window. Click over some different patch types (sports fields, mangroves, ocean, beach, houses) to see how the spectral profile changes. In the inspector section will see change in bands in the form of Histogram in different scenerios.

```js
//To look at this image, we need to add it to our mapping environment.
//Code specifies that for a true colour image, bands 4,3 and 2 should be used in the RGB composite. 
// Sentinel-2 image, which is at 10m resolution for the selected bands
var trueColor = {
  bands: ["B4", "B3", "B2"],
  min: 0,
  max: 3000
};

// Add the image to the map, using visualiation parameters
Map.addLayer(image, trueColor, "True Color Image");
```
Output:                                                         

![alt text](image-16.png)      
Now let's have a look at a false colour composite - we need to bring in the near-infrared band (band 8) for this.       

False-colour composites place the near infra-red band in the red channel, and we see a strong response to the chlorophyll content in green leaves. Vegetation that appears dark green in true colour, appearing bright red in the false-colour. Note the variations in red that can be seen in the vegetation bordering Rapid Creek. You will also see that "false-colour composite" has been added to the Layers tab in the map view.

```js
var falseColor = {
  bands: ["B8", "B4", "B3"],
  min: 0,
  max: 3000
};

// Add the image to the map, using visualiation parameters
Map.addLayer(image, falseColor, "False Color Image", false);
```
Output:                                                         

![alt text](image-17.png)

### Calculating NDVI
Next, let's calculate the normalised-difference vegetation index (NDVI) for this image. NDVI is an index calculated from the RED and NIR bands, according to this equation:                       
***NDVI = (NIR - RED)/(NIR + RED)***
```js
//Define variable NDVI from equation
var NDVI = image.expression(
        "(NIR - RED) / (NIR + RED)",
        {
          RED: image.select("B4"),    //  RED
          NIR: image.select("B8"),    // NIR
          BLUE: image.select("B2")    // BLUE
        });

    Map.addLayer(NDVI, {min: 0, max: 1}, "NDVI", false);
```
Output:                                                       
![alt text](image-18.png)

## YouTube Tutorial
![alt text](image-21.png)
[Click Here](https://youtu.be/P5fgt_lr-HY?si=UVfleh_105uRlUTb) Youtube video like.
```js
// Initially make satellite view as default.
Map.setOptions('SATELLITE');

// Filter Bhutan border
var Bhutan_Border = ee.FeatureCollection("FAO/GAUL/2015/level0").filter(ee.Filter.eq("ADM0_NAME", "Bhutan"));

// Add Bhutan border to map
Map.addLayer(Bhutan_Border, {}, 'Bhutan Border');

// Filter Chukha district
var Dzongkhag = ee.FeatureCollection("FAO/GAUL/2015/level1").filter(ee.Filter.eq("ADM1_NAME", "Samtse"));

// Add Chukha district to map
Map.addLayer(Dzongkhag, {color: 'white'}, 'Dzongkhag');
Map.centerObject(Dzongkhag, 8)

//Importing the LST image collection, MODIS (Moderate Resolution Imaging Spectroradiometer) 
//Land Surface Temperature (LST) data. Importing daily land surface temperature data from the MODIS instrument
var modis =ee.ImageCollection("MODIS/061/MOD11A1")
.filterDate("2022-01-01", "2022-12-31")
.select('LST_Day_1km') //1km resolution data is selected form MODIS bands area

//converting image collection from kelvin to celcius for each images in the collection with function
 // modis.map will apply function taking  one argument to each image on modis varaible and store result 
 // in modis_kel_2_cel variable 
 var modis_kel_2_cel = modis.map(function(img) // defining anonymous function
 {
   return img
   .multiply(0.02) // Scaled Temperature=Original Temperature×0.02, given in MODIS band section
   .subtract(273.15)// Temperature in Celsius=Scaled Temperature−273.15
 // copy specific property from original image to new image, times stamp 
  // time at which the image is captured.
   .copyProperties(img, ['system:time_start'])
 });

// Create chart of LST temporal Analysis
var Time_Series_Chart = ui.Chart.image.series({// is function where drawing chart with series means bands
  imageCollection: modis_kel_2_cel, //which image data to display
  region: Dzongkhag,// ROI to display
  //reducers are used to aggregate or summarize data within a specified region and time period. 
  //particularly useful when working with image collections, as it allows you to condense the 
  // information from multiple images into a single value or set of values
  //Image collections can contain a large number of images, each with many pixels. Reducers enable us to 
  //summarize this wealth of data into a more manageable form, such as a single value per image or per time period.
  // By applying reducers, we can compute various statistics over the data, such as mean, sum, median, standard 
  // deviation, etc
  reducer: ee.Reducer.mean(), 
  scale: 1000, //converting km to meter
  xProperty: "system:time_start"}) 
  .setOptions({ //ets additional options for the chart.
    title: "LST Temporal Analysis",
    vAxis : {title : "LST Celcius"}
  })
print(Time_Series_Chart)

//Image visulization, one image at a time, will take mean value of the images
// So from Layout setting choose custom, stretch 100%, from pallet add multiple color of interest
// then it can be imported as library with name imageVisParam otherwise each time 
//code is rand have to add pallet. Map.addLayer(mean_image, {}, "Mean Image") was before importing imageVisParam
var mean_image = modis_kel_2_cel.mean().clip(Dzongkhag)

Map.addLayer(mean_image, imageVisParam, "Mean Image")

```
Output:      
Bhutan map with boundry for district level and international Border leve.                                                             
![alt text](image-22.png)                                                 

![alt text](image-23.png)

![alt text](image-24.png)

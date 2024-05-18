# Google Earth Engine(Tutorial)

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
![alt text](image-7.png)                                    ![alt text](image-8.png)                                                                   
```js
Map.addLayer(sent2, 
{
  bands: ['B8', 'B4', 'B3'],
  min: 0,
  max: 3000,
  // cannot apply palette when there is multiple bands
}, 'Sentinel-2 RGB False Color Composite', false); // false will uncheck the layers so user can add layer as per requirement
```

## DEM Processing Code <br>
*written for the [Polar Geospatial Center](https://www.pgc.umn.edu/)*

**Project description:** Code written for working with DEM produced by the Polar Geospatial Center. Some functionality is highlighted below"

### Create a *No Data Mask* --- [valid_data.py](https://github.com/jeff-diz/dem_processing/blob/master/lib/valid_data.py)
As the PGC uses the stereo satellite imagery to create DEMs, there are sometimes areas of *No Data* in the resulting DEM. This is usually in areas of water, clouds, or shadow, where the point-matching algorithm (the PGC uses [SETSM](https://github.com/setsmdeveloper/SETSM) could not locate match points, or post-processing filters removed bad data or artifacts. This script includes a function to open a single band raster using GDAL and identify the areas of *No Data*. This function returns a count of valid data pixels and a total count so that users can get an idea of the quality of the DEM without needing to load it into a point and click GIS, compute statistics, set visualization, etc. The script includes options to write the valid data areas out as a binary raster.

```python
def valid_data(gdal_ds, band_number=1, write_valid=False, out_path=None):
    """
    Takes a gdal datasource and determines the number of
    valid pixels in it. Optionally, writing out the valid
    data as a binary raster.
    gdal_ds      (osgeo.gdal.Dataset):    osgeo.gdal.Dataset
    write_valid  (boolean)           :    True to write binary raster, 
                                          must supply out_path
    out_path     (str)               :    Path to write binary raster

    Writes 
    (Optional) Valid data mask as raster

    Returns
    Tuple:  Count of valid pixels, count of total pixels
    """
    # Get raster band
    rb = gdal_ds.GetRasterBand(band_number)
    no_data_val = rb.GetNoDataValue()
    arr = rb.ReadAsArray()
    # Create mask showing only valid data as 1's
    mask = np.where(arr!=no_data_val, 1, 0)
    # Count number of valid
    valid_pixels = len(mask[mask==1])
    total_pixels = mask.size
    
    # Write mask if desired
    if write_valid is True:
        if len(mask.shape) == 2:
            rows, cols = mask.shape
            depth = 1
        else:
            rows, cols, depth = mask.shape
        driver = gdal.GetDriverByName('GTiff')
        
        dst_ds = driver.Create(out_path, ds.RasterXSize, ds.RasterYSize, 1, rb.DataType)
        dst_ds.SetGeoTransform(ds.GetGeoTransform())
        out_prj = osr.SpatialReference()
        out_prj.ImportFromWkt(ds.GetProjectionRef())
        dst_ds.SetProjection(out_prj.ExportToWkt())
        for i in range(depth):
            b = i+1
            dst_ds.GetRasterBand(b).WriteArray(mask)
            dst_ds.GetRasterBand(b).SetNoDataValue(no_data_val)
        dst_ds = None

    return valid_pixels, total_pixels
```

### 2. Assess assumptions on which statistical inference will be based

```javascript
if (isAwesome){
  return true
}
```

### 3. Support the selection of appropriate statistical tools and techniques

<img src="images/dummy_thumbnail.jpg?raw=true"/>

### 4. Provide a basis for further data collection through surveys or experiments

Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo. 

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

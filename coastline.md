## Imagery Selection Schema for Polar Coastline Mapping <br>
*An imagery selection schema based on imagery metadata and NSDIC sea-ice concentration for automated Arctic coastline mapping*

The Polar Geospatial Center is developing a dynamic, automated coastline mapping workflow based on high-resolution, multi-spectral satellite imagery. The basic idea is to use the NIR band to estimate the probability of any given pixel being water, combining these probability rasters over a number of images from a similar timeframe, and extracting a coastline.

However, polar regions obviously have large amounts of sea-ice. Imagery during ice-in becomes useless for this coastline extraction algorithm. While we could estimate the likelihood of an image containing sea-ice just by the date the image was acquired, this might lead to both throwing away good data and incorporating ice-on image.

The [National Snow and Ice Data Center (NSIDC)](https://nsidc.org/) creates daily sea-ice concentration maps with a 25km resolution. This imagery selection schema assesses the sea-ice concentration for each image in the archive by sampling the NSIDC sea-ice concentration daily map corresponding to the date each image was acquired.

[Coastline Imagery Selection](pdf\coastline_selection.pdf) 
[Repository](https://github.com/jeff-diz/coastline)

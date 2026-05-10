# Trend Raster Hotspot Analysis
A fully reproducible ArcPy workflow for creating meaningful hotspot analyses of trend rasters, in particular those that show trends in land cover change over time. Users can batch process datasets and iteratively test which aggregation and analysis parameters best reflect the spatial scale of the dataset's trends.   

The current version of the tool is 1.2.

## Instructions

Place the .ipynb file directly inside an ArcGIS Project folder, next to your project file. Create an "inputs" folder in the same directory, and add your AOI shapefile and input raster(s) to that "inputs" folder. Add the notebook to your project, edit the "User Configuration" cell, and run all cells. See markdown notes in the jupyter notebook for more details.

## Background

This project began when my advisor, Dr. Adriana Uscanga, requested a hotspot analysis of trends in urban greening. The raw trend data was produced by time series analysis of Landsat-based multispectral indices using the LandTrendr algorithm. This would serve as a preliminary visualization for a group of researchers examining relationships between greening trends and urban biodiversity, and we anticipated that the workflow would come in handy for other land cover change trend analysis.   

ArcGIS Pro has out-of-the box geoprocessing tools for hotspot analysis, including both optimized and custom Getis-Ord Gi* tools, but the decisions involved in producing hotspots that provide meaningful insights about patterns in your data can require a fair amount of trial and error. In particular, when analyzing raster data, you must first choose the shape and size of tessellated polygons within which to aggregate the data. You then need to choose an appropriate scale for statistical analysis, e.g. the distance band, number of nearest neighbors, . The Optimized Hotspot Analysis tool will make some of these decisions for you automatically. However, this tool is designed for analyzing spatially irregular incidence point data ("Where are points clustered?") and isn't suited for spatially regular attribute data ("Where are values clustered?"). 

Another colleague, Celia Cortopassi, developed a custom LandTrendr workflow that produced rasters for multiple spectral trends and timescales. These rasters have 30m pixels with a single band value representing the magnitude of spectral change. We decided to use a hexagon mesh for aggregation, and we knew we needed hexagons that could aggregate at least a few pixels in order to see neighborhood-level patterns. We weren’t sure of the exact hexagon size, though, nor how many nearest neighbors we should use for statistical analysis. I remember my advisor warning me: you might have to try it a lot of times. 

To automate the trial-and-error and allow us to repeat our methods with multiple datasets, I decided to create an ArcPy notebook that could batch process rasters with different parameters and let us easily compare all the outputs.

## The Workflow

- The user is asked to specify a list of input rasters, a list of hexagon:pixel ratios (by width), and a list of numbers of nearest neighbors. 
- The rasters are reprojected if needed, aligned, and cropped. 
- Hexagon tessellations are created for each ratio specified, and the width and area of these hexagons are reported in meters. 
- The pixel values within each hexagon are summed and output to a zonal statistics table, then joined to the hexagons. 
- These hexagons are then used as input for a series of Getis-Ord Gi* custom hotspot analyses with each listed number of nearest neighbors. K nearest neighbors is chosen for the spatial relationship because the data has already been aggregated into evenly sized polygons. 
- The hexagons with zonal sums and the hotspot analyses are saved to output folders within the project folder as GeoTIFFs, while intermediate outputs are saved to the project's default geodatabase.

This notebook is currently in its second revision (version 1.2). The next set of improvements I plan to implement include:
- Reducing the quantity of printed messages, unless the user chooses to run in debugging mode 
- Allowing the user more flexibility in choosing output locations and the option to delete intermediate products automatically 
- Reporting the approximate number of city blocks within each hexagon, alongside the hexagon width and area (block size may be user specified)
- Adding more choices of hotspot parameters. 

I look forward to maintaining and improving this tool, and I hope it will find more applications for studying the world's changing landscapes. If you have any feedback you'd like to share, please [contact me](https://cleoyoung.com/contact.html). 
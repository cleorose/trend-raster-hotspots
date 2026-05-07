# Trend Raster Hotspot Analysis
A fully reproducible ArcPy workflow for creating meaningful hotspot analyses of trend rasters, in particular those that show trends in land cover change over time. Users can batch process datasets and iteratively test which aggregation and analysis parameters best reflect the spatial scale of the dataset's trends.   

## How to Use

Place the .ipynb file directly inside an ArcGIS Project folder, next to your project file. Create an "inputs" folder in the same directory, and add your AOI shapefile and input raster(s) to that "inputs" folder. Add the notebook to your project, edit the "User Configuration" cell, and run all cells.   

The current best version of the tool is version 1.2. It is still in its infancy and could use many improvements, but it works. See markdown notes in the jupyter notebook for more details.

## Background

In the fall of 2025, my advisor, Dr. Adriana Uscanga, made a request. She wanted a hotspot analysis of trends in urban greening, as measured by many years of multispectral satellite imagery processed with the LandTrendr algorithm. This would serve as a preliminary visualization for a group of researchers examining relationships between greening trends and urban biodiversity, but similar methods would come in handy for analyzing other trends in land cover change.   

ArcGIS Pro has plenty of pre-made options for running hotspot analysis, but they’re not guaranteed to produce hotspots that give meaningful insights about the data. There are many decisions that go into first aggregating the data and then analyzing the statistical patterns within that aggregation. My advisor explained that we want to use a hexagon fishnet for aggregation, and because we were trying to see neighborhood and city-level patterns in the trends, we needed at least a few pixels per hexagon. We weren’t sure how many, though, nor how many nearest neighbors. I remember her warning me: you might have to try it a lot of times.    

To save myself the misery, and because I was curious, I decided to create an ArcPy notebook that would allow me to batch process rasters with different parameters and then examine all the outputs next to each other. The workflow allows users to choose the ratio(s) of the hexagons to the automatically-detected pixel size of the rasters. The rasters are aligned, projected, and cropped, then multiple hexagon meshes are created based on the specified ratios. The script reports the width and area of the hexagons in meters, allowing the user to understand how their aggregation relates to both the resolution of the source imagery **and** the spatial scale of the landscape. Hotspot analyses are then run with a variable number of nearest neighbors.   

More options for custom parameters will be added in the future. I hope this tool will assist those monitoring and studying the world's changing forests and other ecosystems. 

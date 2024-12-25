# Land Cover Influence on NYC Block Temperatures (WIP)

This project aims to understand the influence of land cover and building height on temperature at the city block level in New York City. By using high-resolution datasets, it seeks to accurately predict temperature for locations without sensors based on environmental factors and visualize temperature variation across the city. This work aims to recreate the project outlined in the article: [*The Urban Heat Island Effect in NYC*](https://a816-dohbesp.nyc.gov/IndicatorPublic/data-stories/urban-heat-island/).

## Data

The data was obtained directly from the NYC OpenData Portal. The following datasets were used in this project:

- [Land Cover Raster Data (2017) – 6in Resolution](https://data.cityofnewyork.us/Environment/Land-Cover-Raster-Data-2017-6in-Resolution/he6d-2qns/about_data)
- [Building Footprints (Map)](https://data.cityofnewyork.us/City-Government/Building-Footprints-Map-/3g6p-4u5s)
- [Hyperlocal Temperature Monitoring](https://data.cityofnewyork.us/dataset/Hyperlocal-Temperature-Monitoring/qdq3-9eqn/about_data)

## Files

### FulllandCoverCharacteristics

The final CSV file, produced in NYC_LandCover_dataframe and combined in NYC_LandCover_df_merge, contains combined land cover data from multiple individual CSV files. Each row in the dataset represents information about a specific sensor's location, such as its ID, latitude, and longitude, along with various environmental characteristics within a defined buffer around that sensor. The dataset includes percentages for different land cover types, such as tree canopy, grass and shrubs, bare soil, water, buildings, roads, other impervious surfaces, and railroads. Additionally, building characteristics, such as average and median building heights, as well as the proportions of buildings categorized by height (low, medium, tall), are included. The dataset also contains building age statistics, with means and medians based on construction years, if available. All missing data in the combined dataset is replaced by zeros to ensure completeness for analysis. This data serves as a comprehensive overview of land cover and urban infrastructure features for the monitored locations.

| Column Name        | Description                                                                                  |
|--------------------|----------------------------------------------------------------------------------------------|
| Sensor.ID          | A unique identifier for each sensor or data point in the dataset.                             |
| Latitude           | The latitude coordinate of the sensor location.                                              |
| Longitude          | The longitude coordinate of the sensor location.                                             |
| Buffer             | The buffer area around the sensor, typically a radial distance for analysis.                  |
| TreeCanopy_pct     | Percentage of the area covered by tree canopy within the buffer zone.                        |
| GrassShrubs_pct    | Percentage of the area covered by grass and shrubs within the buffer zone.                   |
| BareSoil_pct       | Percentage of the area covered by bare soil within the buffer zone.                          |
| Water_pct          | Percentage of the area covered by water bodies within the buffer zone.                       |
| Buildings_pct      | Percentage of the area covered by buildings within the buffer zone.                          |
| Roads_pct          | Percentage of the area covered by roads within the buffer zone.                              |
| OtherImp_pct       | Percentage of the area covered by other impervious surfaces within the buffer zone.          |
| Railroads_pct      | Percentage of the area covered by railroads within the buffer zone.                          |
| Building_mean      | Mean value of building heights (based on roof height) within the buffer zone.                |
| Building_median    | Median value of building heights (based on roof height) within the buffer zone.              |
| BuildingAge_mean   | Mean age of buildings within the buffer zone.                                                |
| BuildingAge_median | Median age of buildings within the buffer zone.                                              |
| BuildingLow_pct    | Percentage of low-rise buildings (heightroof <= 20) within the buffer zone.                  |
| BuildingMedium_pct | Percentage of medium-rise buildings (heightroof <= 40) within the buffer zone.               |
| BuildingTall_pct   | Percentage of tall buildings (heightroof > 40) within the buffer zone.                       |

### NYC_LandCover_Plots

This function, plot_landcover_with_buildings, creates a map that visualizes land cover, building footprints, and temperature sensor locations within a specified buffer zone (around a given point, defined by longitude and latitude). The function takes the following inputs:

Coordinates (longitude, latitude)
Buffer distance
Land cover raster
Building footprint data (in GeoJSON format)
Temperature data (in a dataframe)
An optional output file for saving the plot
The function first transforms the input data to match the coordinate reference system (CRS) of the land cover raster. It then creates a buffer around the specified point, crops the raster to this buffer, and processes building data by classifying buildings by height. Temperature sensor data is also clipped and reprojected to the relevant area. The map is generated using ggplot2, layering the land cover, buildings, and sensor data with distinct colors for easy visualization. Finally, the plot is either displayed on the screen or saved as an image file.

### NYC_LandCover_dataframe

This R code is designed to process and analyze spatial data, including land cover, building footprints, and temperature readings, specifically for New York City. It begins by loading essential libraries and importing data from several sources: land cover raster data, building footprints in GeoJSON format, and temperature sensor data.

A key function, generate_landcover_and_building_data, extracts relevant spatial data based on specified coordinates. It then applies spatial buffering and processes the data to calculate land cover percentages and building characteristics. The function also computes building height categories and, where available, building age metrics.

The code uses a loop to process temperature sensor data, applying this function to each sensor's location. For each sensor, it calculates land cover percentages (e.g., tree canopy, grass, bare soil) and building metrics (e.g., average building height, building height categories). It also computes building age metrics where available. The results are compiled into a dataframe and saved as CSV files for each sensor.

To efficiently process large datasets, the code works in chunks. It processes a specified number of sensors per chunk (e.g., 20), saving progress along the way to prevent data loss. If chunks have already been processed, the code skips them. This ensures the program handles large datasets without overloading the system, and once all chunks are processed, it confirms that all data has been saved successfully.

### NYC_LandCover_df_merge

This R code merges multiple CSV files containing land cover data into a single dataframe. It starts by loading the dplyr library and specifying the directory containing the CSV files. The code then lists all the CSV files in the directory, reads them into a list, and combines them into one dataframe using bind_rows(). Missing (NA) values in the dataset are replaced with zeros to ensure completeness.

Although the code includes a line to save the merged dataframe as a CSV (which is currently commented out), it ensures the data is processed and combined efficiently, preparing it for further analysis.

## Requirements

- [RStudio](https://posit.co/download/rstudio-desktop/)

### Packages

- [sf](https://cran.r-project.org/web/packages/sf/index.html)
- [raster](https://cran.r-project.org/web/packages/raster/index.html)
- [dplyr](https://dplyr.tidyverse.org/)
- [ggplot2](https://ggplot2.tidyverse.org/)
- [ggnewscale](https://cran.r-project.org/web/packages/ggnewscale/index.html)
- [lubridate](https://lubridate.tidyverse.org/)

## Next Steps

- Perform Exploratory Data Analysis (EDA) on the dataset to better understand its structure, distributions, and relationships between variables. This will include examining summary statistics, visualizing correlations, and identifying any patterns or potential issues with the data.
- Apply a RandomForestRegressor model to predict temperature based on land cover and building characteristics. This will involve training the model with relevant features, such as land cover types and building data, to understand their impact on temperature variation.
- Create an interactive dashboard that allows users to explore various land cover characteristics and visualize their effect on temperature. This dashboard will enable users to manipulate different parameters and see how changes in land cover influence temperature predictions.

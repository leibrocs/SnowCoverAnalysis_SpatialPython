# Analysis of Snow Cover and Snow Cover Duration in Central Asia

## 1. Introduction
Snow is one of the essential factors in the cryosphere and very sensitive to global climate change, especially in the arid and semi-arid regions of Central Asia. Locally and regionally, snow limits wild animals' wintertime access to forage, influencing migration and hibernation. Seasonal snow cover acts as efficient insulation as well as a natural water reservoir, storing solid precipitation in winter and releasing it during the spring and summer melt. Across the world, more than a billion people rely on snow as a resource for drinking water, irrigation, and/or hydroelectric power. Significant changes in how long snow persists on the ground can change the timing and amount of available water for communities dependent on snowmelt. Snow cover is measured in duration, meaning how many days a particular place is covered by snow. This analysis investigated how long the snow covered lasted each year and if that behavior has been changing for ten hydrological years from 2012 to 2022 for a study region loacted in Central Asia.


## 2. Material and Methods

### Study Region - Central Asia
The study region was defined by the MODIS tile h23v04, which is located in Central Asia and covers the corner region of seven countries (Russia, Mongolia, Kazakhstan, Uzbekistan, Kyrgyzstan, Tajikistan, and China). It includes the entire Tianshan Mountains in the South as well as the Altai Mountain Range in the North with the Zhunge’er Basin in between. Over 80% of the precipitation concentrates on the mountainous areas, providing crucial water resources for this arid and semiarid region. The snow cover and snow cover accumulation differs significantly due to complex topography, various land types as well as varying climate regimes.

<p align="center">
<img src="https://github.com/leibrocs/SnowCoverAnalysis_SpatialPython/assets/116877154/123a0afe-6219-4f9b-afac-4983be31216b" width="600"/>
<p/>


### Data
For this analysis MODIS Terra and Aqua Snow Cover Daily L3 Global 500m SIN Grid v61 data from the National Snow and Ice Data Center (NSIDC) was used. The snow cover in the product was identified using the Normalized Difference Snow Index (NDSI). The study period covered ten hydrological years starting from the first of September 2012 to the 31st of August 2022. For every hydrological year 365 tiles were analysed except for the leap years 2016 and 2020, where the year consisted of 366 scenes. In total, 7.304 MODIS Terra and Aqua scenes were downloaded. Additionally, a SRTM covering the study region was used.


### Data Preparation

#### Data Download
To acquire ten years of MODIS snow cover data from the two satellites automatically, the script Download_MODIS.ipynb had to be run twice. Once to download the MODIS Terra snow cover data and a second time to get the scenes from the aqua satellite. As input a GeoJSON file with the corner coordinates of the study region was required. Additionally, the time frame needed to be specified to filter the granules and was set from first of September 2012 to 31st of August 2022. 


#### Replace Missing Files
Some MODIS terra Snow Cover files were missing from the ten-year time series and needed to be identified and replaced. The script ReplaceMissing_MODIS.ipynb runs through the terra file folder, detects missing files and replaces them with the respective files from the aqua folder. Additionally, in case that there was neither a terra nor an aqua file, a new file only containing zeros was created and stored in the same folder.

After the terra file folder was updated and included the complete ten-year daily time series of MODIS snow cover data, the folder was manually split into multiple subfolders containing only the data for one hydrological year. This resulted in ten folders with 365 or 366 (for leap years) files. This step was necessary because the used laptop was not able to handle a large dataset of the size of the entire MODIS terra folder.

  -> Example subfolder name: MOD10A1_12-13, MOD10A1_13-14, ..., MOD10A1_21-22
  
Additionally, a snow cover output folder was created for every hydrological year to later store the GeoTiff datasets with the snow/ no snow information.

  -> Example snow cover folder name: SC_12-13, SC_13-14, ..., SC_21-22

### Snow Cover and Snow Cover Duration (SCD) Analysis
After the data preparation, one terra subfolder after another was read in using the SnowCover_SnowCoverDuration.ipynb script. It was used to clear all cloud- and no-data flagged pixels and to interpolate these data/ cloud gaps to identify the snow cover status on the ground. The resulting raster files were then stored in a previously defined snow cover output folder for the respective hydrological year. After the calculation of the snow cover for every terra scene, the snow cover duration for the currently open year was determined for each pixel, which resulted in a raster with SCD values ranging from 0 to 365/ 366. The output file was then stored in a separate folder, where all the SCD raster files for the ten hydrological years were saved.
Lastly, the SCD was visualized for every 250-meter elevation zone using the script SCD_Visualization.ipynb. It was also used to calculate the mean SCD and to plot the mean SCD above 400 meters.

## 3. Results

### Cloud Cover
The study region was often heavily covered by clouds, with an average cloud cover percentage of ~ 45%. During the middle of the hydrological year the most clouds occured which resulted in scenes that were close to 100% covered by clouds. In the beginning and the end of the study period, which corresponds to the summer months, the lowest cloud cover percentage was observable.

<p align="center">
  <img width="606" alt="CloudCover_21-22" src="https://github.com/leibrocs/SnowCoverAnalysis_SpatialPython/assets/116877154/e10aebdd-36c7-4623-a1ca-21ec598515aa">
</p>

### Snow Cover Duration 
The SCD was calculated for every hydrological year and every pixel of the MODIS scenes and ranged from 0 to 365/ 366 days.

<p align="center">
<img width="500" alt="MeanSCD_16bit" src="https://github.com/leibrocs/SnowCoverAnalysis_SpatialPython/assets/116877154/8f9be2d5-dc7e-4324-a5fe-10c263e2a5fe">
<p/>

### Elevation Dynamics of the Snow Cover Duration
To analyse the dynamics of the SCD for different elevation zones, the it was plotted for each hydrological year and for 29 250-meter elevation zones ranging from zero to 7.000 meters. The results suggest that the number of days with snow cover increases along the elevation profile. For low elevations the mean SCD increased strongly with each elevation zone. Between 750 and 4.500 meters the SCD increase with elevation slows down to durations ranging from ~80 to ~170 days. Above 4.500 meters the SCD is again rapidly rising to durations up to 365 days in the highest elevation zone. Additionally, there were fluctuations between the SCDs for the different elevation zones over the ten years observable. For this time series, an overall increasing or decreasing trend in the number of snow-covered days per year is not present.

<p align="center">
<img width="593" alt="MeanSCD_250m_ElevationZones_16bit" src="https://github.com/leibrocs/SnowCoverAnalysis_SpatialPython/assets/116877154/687c82e2-18c7-4138-af9d-4ff3143515cb">
<p/>

Above 400 meters the mean SCD was fluctuating between 98 days in 21/22, and 131 days in 16/17. The average amount of snow cover days over the ten-year time series was 110.

<p align="center">
<img width="500" alt="MeanSCD_above_400m_16bit" src="https://github.com/leibrocs/SnowCoverAnalysis_SpatialPython/assets/116877154/89034fdc-0ff8-4c2a-a5c5-6fada960a43e">
<p/>

### Visualization of the Mean SCD in QGIS
<p float="left">
  <img width="500" alt="MeanSCD_QGIS_16bit" src="https://github.com/leibrocs/SnowCoverAnalysis_SpatialPython/assets/116877154/0efd159a-96dc-49ee-a634-6351617522ca"/>
  <img width="500" src="https://github.com/leibrocs/SnowCoverAnalysis_SpatialPython/assets/116877154/27799207-6a14-4a0c-bfa0-4543257e0b12"/>
<p/> 
  
Lastly, the mean SCD over the whole time series of ten years was visualized using QGIS software. The first map displays nicely the increase in SCD with elevation. The highest values were found at the peaks of the Tianshan Mountain range in the South of the study region which corresponds to the areas with the highest elevation. Additionally, both maps show, that the areas with zero SCD are all located at water bodies. In the second map the slow gain in SCD towards the mountainous areas can be seen. Especially in valleys close to the mountains the SCD seems to be particularly low.

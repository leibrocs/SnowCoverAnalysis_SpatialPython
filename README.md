# Analysis of Snow Cover and Snow Cover Duration in Central Asia

## 1. Introduction
Snow is one of the essential factors in the cryosphere and very sensitive to global climate change, especially in the arid and semi-arid regions of Central Asia [1]. Seasonal snow cover acts as efficient insulation as well as a natural water reservoir, storing solid precipitation in winter and releasing it during the spring and summer melt. Across the world, more than a billion people rely on snow as a resource for drinking water, irrigation, and/or hydroelectric power. Significant changes in how long snow persists on the ground can change the timing and amount of available water for communities dependent on snowmelt [2]. Snow cover is measured in duration, meaning how many days a particular place is covered by snow [1]. In remote areas, like Central Asia, situ monitoring is rare due to terrain complexity and inaccessibility, making remote sensing the most practical way to understand snow pattern. The Moderate Resolution Imaging Spectrometer (MODIS) is a popular choice for assessing snow cover trends because of its multispectral band placement, radiometric resolution that reduces saturation over bright snow, frequent temporal resolution, and record length, with snow cover maps since 1999/2002 (Aqua/Terra). This analysis used the daily MODIS Terra and Aqua Snow Cover product to investigated snow cover pattern and snow cover duration for a period of ten hydrological years from 2012 to 2022 in Central Asia.


## 2. Material and Methods

### Study Region - Central Asia
The study region was defined by the extent of the MODIS tile h23v04, which is located in Central Asia and covers the corner region of seven countries (Russia, Mongolia, Kazakhstan, Uzbekistan, Kyrgyzstan, Tajikistan, and China). It includes the entire Tianshan Mountains in the South as well as the Altai Mountain Range in the North with the Zhunge’er Basin in between. Over 80% of the precipitation concentrates on the mountainous areas, providing crucial water resources for this arid and semiarid region. The snow cover and snow cover accumulation in this area differ significantly due to complex topography, various land types as well as varying climate regimes [3].

<p align="center">
<img src="https://github.com/leibrocs/SnowCoverAnalysis_SpatialPython/assets/116877154/123a0afe-6219-4f9b-afac-4983be31216b" width="600"/>
<p/>


### Data
For this analysis MODIS Terra and Aqua Snow Cover Daily L3 Global 500m SIN Grid v61 data from the National Snow and Ice Data Center (NSIDC) was used. The snow cover in the product was identified using the Normalized Difference Snow Index (NDSI). The study period covered ten hydrological years starting from the first of September 2012 to the 31st of August 2022. For every hydrological year 365 tiles were analysed except for the two leap years 2016 and 2020, where the year consisted of 366 scenes. In total, 7.304 MODIS Terra and Aqua scenes were downloaded. Additionally, a SRTM covering the study region was used.


### Data Preparation

#### Data Download
To automatically acquire ten years of MODIS snow cover data from the two satellites, the script "Download_MODIS.ipynb" had to be executed twice. The first run was performed to download the MODIS Terra snow cover data, and the second run was carried out to obtain scenes from the Aqua satellite. As input, a GeoJSON file containing the corner coordinates of the study region was required. Additionally, a time frame needed to be specified to filter the granules, which was set from September first 2012 to August 31st of 2022.


#### Replace Missing Files
Certain MODIS Terra Snow Cover files were found to be missing within the ten-year time series and needed identification and replacement. The script "ReplaceMissing_MODIS.ipynb" was used to run through the Terra file folder, detect the absence of files, and substitutes them with the corresponding files from the Aqua folder. Furthermore, if neither a Terra nor an Aqua file was available, a new file containing only zeros was generated and stored in the same folder.

Following the update of the Terra file folder, which now included the complete ten-year daily time series of MODIS Snow Cover data, the folder was manually divided into multiple subfolders, each containing data for a single hydrological year. This resulted in ten folders, each containing either 365 files or 366 files for leap years. This division was imperative as the laptop in use could not efficiently manage a large dataset of the entire size of the MODIS Terra folder.

  -> Example subfolder name: MOD10A1_12-13, MOD10A1_13-14, ..., MOD10A1_21-22
  
Additionally, a snow cover output folder was created for every hydrological year to later store the GeoTiff datasets with the snow/ no snow information.

  -> Example snow cover folder name: SC_12-13, SC_13-14, ..., SC_21-22

### Snow Cover and Snow Cover Duration (SCD) Analysis
Following the data preparation phase, the "SnowCover_SnowCoverDuration.ipynb" script was employed to process one Terra subfolder at a time. Its purpose was to eliminate any cloud- or no-data flagged pixels and perform interpolation to fill data gaps, ultimately identifying the snow cover status on the ground. The resulting raster files were subsequently stored in a snow cover output folder that had been defined in advance, corresponding to the respective hydrological year.

Upon completing the snow cover calculation for each Terra scene, the snow cover duration (SCD) for the currently processed year was determined for every pixel, resulting in a raster file containing SCD values ranging from 0 to 365 (or 366 for leap years). These output files were stored in a separate folder, containing all SCD raster files for the ten hydrological years.

Finally, the SCD data were visualized for each 250-meter elevation zone using the "SCD_Visualization.ipynb" script. This script was also employed to calculate the mean SCD values and generate plots illustrating the mean SCD values for elevations above 400 meters.


## 3. Results

### Cloud Cover
The study region frequently experienced significant cloud cover, with an average cloud cover percentage of approximately 45%. The highest incidence of clouds occurred during the midpoint of the hydrological year, leading to scenes that were nearly 100% obscured by clouds. Conversely, during the initial and final phases of the study period, corresponding to the summer months, the cloud cover percentage was at its lowest.

<p align="center">
  <img width="606" alt="CloudCover_21-22" src="https://github.com/leibrocs/SnowCoverAnalysis_SpatialPython/assets/116877154/e10aebdd-36c7-4623-a1ca-21ec598515aa">
</p>

### Snow Cover Duration 
The SCD was calculated for every hydrological year and every pixel of the MODIS scenes and ranged from 0 to 365/ 366 days.

<p align="center">
<img width="500" alt="MeanSCD_16bit" src="https://github.com/leibrocs/SnowCoverAnalysis_SpatialPython/assets/116877154/8f9be2d5-dc7e-4324-a5fe-10c263e2a5fe">
<p/>

### Elevation Dynamics of the Snow Cover Duration
To analyze the dynamics of the SCD across various elevation zones, it was plotted for each hydrological year over 29 250-meter elevation zones spanning from zero to 7,000 meters. The results indicate a trend where the number of days with snow cover increases with rising elevation. At lower elevations, the mean SCD experiences a notable increase with each elevation zone. Between elevations of 750 and 4,500 meters, the rate of increase in SCD with elevation becomes more gradual, resulting in durations ranging from approximately 80 to about 170 days.

However, above the elevation of 4,500 meters, SCD starts to rapidly rise again, reaching durations of up to 365 days in the highest elevation zone. Additionally, noticeable fluctuations in SCD were observed among the different elevation zones over the ten-year period. Notably, there was no consistent overall trend of increasing or decreasing snow-covered days per year in this time series analysis.

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
  
Lastly, the mean SCD over the entire ten-year time series was visualized using QGIS software. In thefirst map, a clear pattern emerged, illustrating the increase in SCD with higher elevations. The highest SCD values were observed at the peaks of the Tianshan Mountain range in the southern part of the study region, which corresponds to the areas with the greatest elevation. Additionally, both maps revealed that regions with zero SCD were consistently situated over water bodies. 
In the second map, a gradual and steady rise in SCD towards the mountainous areas became evident. Particularly in valleys near the mountains, the SCD appeared to be notably lower.

## 4. References
[1] Liu, J. P.; Zhang, W. C. (2017): Long term spatio-temporal analyses of snow cover in Central Asia using ERA-Interim and MODIS products. In: IOP Conf. Ser.: Earth Environ. Sci. 57, S. 12033. DOI: 10.1088/1755-1315/57/1/012033.

[2] Ackroyd C, Skiles SM, Rittger K and Meyer J (2021) Trends in Snow Cover Duration Across River Basins in High Mountain Asia From Daily Gap-Filled MODIS Fractional Snow Covered Area. Front. Earth Sci. 9:713145. doi: 10.3389/feart.2021.713145.

[3] Wang, Xianwei; Xie, Hongjie; Liang, Tiangang; Huang, Xiaodong (2009): Comparison and validation of MODIS standard and new combination of Terra and Aqua snow cover products in northern Xinjiang, China. In: Hydrol. Process. 23 (3), S. 419–429. DOI: 10.1002/hyp.7151.

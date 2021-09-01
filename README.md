# CQuest Challenge 

## Contents : 
* [Data Information](#data-information)
* [Understanding the Data set](#understanding-the-data)
* [Merging and finding correlations](#merging-and-finding-correlations)
* [References](#references)

### **Data Information**

* The Data has been taken from Natural Resources Conservation Service Soils (United States Department of Agriculture) [[1]](#1)
* Data Description
  * `RaCA_data_columns_expl` - This data gives the information about the columns of the general location and pedons dataset and most importantly the breakdown of the RaCA side ID code
  * `RaCA_general_location` - This data gives the coordinates (exact locations) of the RCA Site IDs. 
  * `RaCA_samples` - This data contains all the samples of the pedons of each RCA site id with the soil properties in it.
  * `RaCA_SOC_pedons` - This data contains the information of pedons and number of samples which were taken from each pedon. 
* `What are Pedons?` - Pedons are three-dimensional bodies of soil with lateral dimensions large enough to permit the study of horizon shapes and relations. Here a three-dimensional sample of a soil just large enough to show the characteristics of all its horizons. [[2]](#2) 
* `What is a SOC?` - Soil Organic Carbon, read more here - Soil organic carbon (SOC) refers only to the carbon component of organic compounds. Soil organic matter (SOM) is difficult to measure directly, so laboratories tend to measure and report SOC. [[3]](#3)
<p align="center">
  <img src="https://user-images.githubusercontent.com/75158219/131504716-c6b425c1-a4d5-45e0-aaed-e0eccaabe252.png" alt="Sublime's custom image"/>
</p>   
<p align='center'>
   <em>Picture depecting a Pedon [4]</em>
</p>
  
#### Understanding the RCA Site ID Code 
 
>RaCA site ID - Code\
>RaCA site ID = CxxyyLzz\
>C = placeholder character (C,A,X or F)\
>xx = RaCA Region/old MO number (01 - 18)\
>yy = statistical group # for MO (number varies by region)\
>L = land use/land cover type (C=Cropland, F=Forest land, P=Pastureland, R=Rangeland, W=Wetland, and X=CRP)\
>zz = Plot # within the group

### **Understanding the Data**

#### 1. GeneralLocation Data Study - Please open `1_GeneralLocationDataStudy.ipynb`

1. The best way to start working on data is to know for which locations are you working on. 
2. I imported the csv file into dataframe and converted it to a geodataframe from `data\RaCA_general_location.csv`
3. Using KeplerGl I understood the Points belong to USA, and output can be seen in [here](https://user-images.githubusercontent.com/75158219/131683244-dbadc435-0a8b-456e-bc0a-4cb63201e3fa.png) or in `output\images\1_locationpoints_usa.png`.
4. I processed the Longitude and Latitude of the data, and created a geodataframe with the geometry column and saved the processed out in geojson format for future use and saved the file in `processed_data\general_location_processed.geojson`


#### 2. Pedon Data Study - Please open `2_PedonDataStudy.ipynb`

1. I imported the csv file into dataframe using the pandas library from `data\RaCA_SOC_pedons.csv`
2. I found some identifiers and I removed the duplicate identifiers from the pedons dataframe which were of no use. 
3. I selected only the columns which were needed in the requirement along with the identifiers. 
4. I fetched the Land Use from the upedon column, and using a pie plot understood the distribution of the pedons(samples) from different LandUse and the output can be seen in [here](https://user-images.githubusercontent.com/75158219/131683898-79948414-f380-4db9-b26c-8200686bd474.png) or in `output\images\2_landuse_distribution_pie.png`
5. I plotted the corelation matrix and found out SOCstoc100 and SOCstock30 are highly corelated output can be seen [here](https://user-images.githubusercontent.com/75158219/131683820-3bd82c93-3e6e-4a02-bca2-a18ce7d4514d.png) or in `output\images\2_correlationmatrix_soc.png`
6. I saved the processed dataframe to a csv which will be used further in `processed_data\pedons_processed.csv`


#### 3. Samples Data Study - Please open `3_SamplesDataStudy.ipynb`

1. I imported the csv file into dataframe using the pandas library from `data\RaCA_samples.csv` 
2. I found some identifiers and I removed the duplicate identifiers from the samples dataframe which were of no use.
3. I selected only the columns which were needed in the requirement along with the identifiers. 
4. I found a sample id is duplicated `C0408C011-1` and I discarded the sample. 
5. I saved the processed dataframe to a csv which will be used further in `processed_data\sample_data_processed.csv`

### Merging and Finding Correlations

#### 4. Identifying the common indices to merge the datas. - Please open `4_Merging_Data.ipynb`

1. I have imported the processed data from the `processed_data` folder. 
1. For Merging the _sample_ and _pedon_ dataframe I used the columns `upedon` and `rcasiteid` and named as `samples_pedons_df`
2. For merging the `sample_pedons_df` and the `location_gdf` I used the column `rcasiteid` 
3. I merged all three data and stored it as a geojson format as `processed_data\pedon_sample_location.geojson` file


#### 5. Correlation - Please open `5_Correlation.ipynb`

1. I have imported the processed merged data `processed_data\pedon_sample_location.geojson` file
1. I found the total na values of each column.
2. I took a sample of caco3 and found out the mean for each Land_Use is quite different, so I cannot replace the missing value with the mean of the complete data set.
3. I grouped the data with LandUse and using mean of the series I replaced the fillna.
4. I plotted the correlation matrix of the complete merged dataset which can be seen [here](https://user-images.githubusercontent.com/75158219/131684580-d393a582-958b-43fe-8a85-25fb996f22ec.png) or in `output\images\5_correlation_matrix.png`
5. Using the mean of each SOC (For each LandUse group), I have plottd a stack plot which can be seen [here](https://user-images.githubusercontent.com/75158219/131685104-ffdcdf71-91b9-4a42-ac61-a72058999508.png) or in `output\images\5_stackplot_soc_landuse.png`
6. I have saved the final merged data in different formats (ESRIShape, GeoJSON, CSV and HTML-Kelper) in their respective output folders.
7. I have used KeplerGL package to observe the pattern of the data, and are listed below : 
   * HeatMap of the BOT (Bottom) Column which show the place where the most depth pedons were taken from, the picture can be found [here](https://user-images.githubusercontent.com/75158219/131688808-93ec904e-7b96-472b-8fbe-a08f3f9e1090.png) or in `output\images\heatmap_BOT.png`and the same can be seen in the grid3map [here](https://user-images.githubusercontent.com/75158219/131689654-091723a4-16f5-4f01-a086-6a87ebfc5c8a.png) or in `output\images\grid3dmap_BPT.png`\
   * Radius map of the Bulkdensity and SOCStock100 where the color code will show the bulkdensity and the radius of the point will tell the SOCstock100 content. The pciture can be found [here](https://user-images.githubusercontent.com/75158219/131689073-1a139d3b-7044-41ac-857b-d0c01589463e.png) or in `output\images\radiusmap_bulkdensity_SOCstock100.png`
   * Heat map and the grid3dmap of the c_tot_ncs can be found [here-heatmap](https://user-images.githubusercontent.com/75158219/131690097-c82fbdee-a8a1-45ec-94e5-2b175bc97151.png) and [here-3dmap](https://user-images.githubusercontent.com/75158219/131690561-7c3f719c-7e5a-4018-9660-44e788e62ace.png)
 or in `output\images\heatmap_c_tot_ncs.png` and `output\images\grid3dmap_c_tot_ncs.png`
   * Radius map of the SOCstock100 with the Land_Use can be found [here]![image](https://user-images.githubusercontent.com/75158219/131690698-79fd86a8-59b5-41d7-bb9c-366babac9d94.png)
 or in `output\images\radiumsmap_LandUse_SOCstock100.png`








### References

* <a id="1">[1]</a> 
https://www.nrcs.usda.gov/wps/portal/nrcs/detail/soils/survey/?cid=nrcs142p2_054164#data_tables
* <a id="2">[2]</a> 
https://www.sciencedirect.com/topics/earth-and-planetary-sciences/pedon
* <a id="3">[3]</a> 
https://www.agric.wa.gov.au/measuring-and-assessing-soils/what-soil-organic-carbon#:~:text=Soil%20organic%20carbon%20(SOC)%20refers,to%20measure%20and%20report%20SOC
* <a id="4">[4]</a> 
https://www.researchgate.net/profile/Eyasu-Elias/publication/343450769/figure/fig3/AS:921214222626816@1596645994352/a-Pedon-solum-and-soil-individual-in-a-landscape-b-a-typical-soil-profile-Source.jpg

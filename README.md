# CQuest Challenge 

## Contents : 
* [Data Information](#data-information)
* [Understanding the Data set](#understanding-the-data)
* [Merging and finding corelations](#)
* [References](#references)

### **Data Information** : 

* The Data has been taken from Natural Resources Conservation Service Soils (United States Department of Agriculture) [[1]](#1)
* Data Description
  * `RaCA_data_columns_expl` - This data gives the information about the columns of the general location and pedons dataset and most importantly the breakdown of the RaCA side ID code
  * `RaCA_general_location` - This data gives the coordinates of the RCA Site IDs. 
  * `RaCA_samples` - This data contains all the samples of the pedons of each RCA site id with the soil properties in it.
  * `RaCA_SOC_pedons` - This data contains the information of pedons and number of samples which were taken from each pedon. 
* `Pedons` - Pedons are three-dimensional bodies of soil with lateral dimensions large enough to permit the study of horizon shapes and relations. Here a three-dimensional sample of a soil just large enough to show the characteristics of all its horizons. [[2]](#2) 
* `SOC` - Soil Organic Carbon, read more here - Soil organic carbon (SOC) refers only to the carbon component of organic compounds. Soil organic matter (SOM) is difficult to measure directly, so laboratories tend to measure and report SOC. [[3]](#3)
<p align="center">
  <img src="https://user-images.githubusercontent.com/75158219/131504716-c6b425c1-a4d5-45e0-aaed-e0eccaabe252.png" alt="Sublime's custom image"/>
</p>
Picture Taken From : https://www.researchgate.net/profile/Eyasu-Elias/publication/343450769/figure/fig3/AS:921214222626816@1596645994352/a-Pedon-solum-and-soil-individual-in-a-landscape-b-a-typical-soil-profile-Source.jpg
  
 ```
RaCA site ID - Code
RaCA site ID = CxxyyLzz

C = placeholder character (C,A,X or F)
xx = RaCA Region/old MO number (01 - 18)
yy = statistical group # for MO (number varies by region)
 L = land use/land cover type (C=Cropland, F=Forest land, P=Pastureland, R=Rangeland, W=Wetland, and X=CRP)
zz = Plot # within the group
```

### **Understanding the Data**

#### 1. GeneralLocation Data Study - Open [here](http://localhost:8889/doc/tree/GIS/CQuest_Challenge/1_GeneralLocationDataStudy.ipynb)

1. The best way to start working on data is to know for which locations are you working on. 
2. I imported the csv file into dataframe and converted it to a geodataframe. 
3. Using KeplerGl I understood the Points belong to USA. 
4. I processed the Longitude and Latitude of the data, and created a geodataframe with the geometry column and saved the processed out in geojson format for future use. 


#### 2. Pedon Data Study 

1. I imported the csv file into dataframe using the pandas library. 
2. I found some identifiers and I removed the duplicate identifiers from the pedons dataframe which were of no use. 
3. I selected only the columns which were needed in the requirement along with the identifiers. 
4. I fetched the Land Use from the upedon column, and plotting the number of pedons taken from each Land Use. 
5. I plotted the corelation matrix and found out SOCstoc100 and SOCstock30 are highly corelated. 
6. I calculated the mean SOC values for each depth (5, 30, and 100 cms) for each Land Use. 
7. I saved the processed dataframe to a csv which will be used further. 


#### 3. Samples Data Study

1. I imported the csv file into dataframe using the pandas library. 
2. I found some identifiers and I removed the duplicate identifiers from the samples dataframe which were of no use.
3. I selected only the columns which were needed in the requirement along with the identifiers. 
4. I found a sample id is duplicated `C0408C011-1` and I discarded the sample. 
5. I saved the processed dataframe to a csv which will be used further.

#### 4. Identifying the common indices to merge the datas.

1. For Merging the _sample_ and _pedon_ dataframe I used the columns `upedon` and `rcasiteid`. 
2. For merging the sample_pedon dataframe I used the column `rcasiteid` 
3. I merged all three data and stored it as a geojson format as `pedon_sample_location.geojson` file


#### 5. Corelation. 

1. I found the total na values of each column
2. I took a sample of caco3 and found out the mean for each Land_Use is quite different, so I cannot replace the missing value with the mean of the complete data set.
3. I grouped the data with LandUse and using mean of the series I replaced the fillna
4. 





### References

* <a id="1">[1]</a> 
https://www.nrcs.usda.gov/wps/portal/nrcs/detail/soils/survey/?cid=nrcs142p2_054164#data_tables
* <a id="2">[2]</a> 
https://www.sciencedirect.com/topics/earth-and-planetary-sciences/pedon
* <a id="3">[3]</a> 
https://www.agric.wa.gov.au/measuring-and-assessing-soils/what-soil-organic-carbon#:~:text=Soil%20organic%20carbon%20(SOC)%20refers,to%20measure%20and%20report%20SOC
* <a id="4">[4]</a> 
https://en.wikipedia.org/wiki/Sentinel-2#Spectral_bands
* <a id="5">[5]</a> 
https://sentinel.esa.int/web/sentinel/technical-guides/sentinel-2-msi/level-2a/algorithm
* <a id="6">[6]</a> 
https://www.satimagingcorp.com/satellite-sensors/other-satellite-sensors/sentinel-2a/

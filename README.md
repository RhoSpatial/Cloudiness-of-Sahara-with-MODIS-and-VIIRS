# Cloudiness-of-Sahara-with-MODIS-and-VIIRS

### Clouds area retrieving method:
Imagery analytics where performed on Google Earth Engine platform using java script. NDCI(normalized cloud index)= (sur_refl_b01 - sur_refl_b06)/(sur_refl_b01 + sur_refl_b06).
Imagery processing SDS cloud flag was used is a base for cloud mapping; LOW PASS process NDCI greather than -0.14; MID PASS process NDCI greather than 0; HIGH PASS processes red band surface reflectance greather than 0.6.



### Results:
![Cloudiness by DOY  2000-2023 average (N=8194)](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/cad0f5bb-5c60-4cf6-84f5-bde30f9b35f0)

![Average cloudiness of weeks__march2000-march2023](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/53bb48ee-16e1-4998-a72a-ef9f549f226b)

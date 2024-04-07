# Cloudiness-of-Sahara-with-MODIS-and-VIIRS

### Clouds area retrieving method:
Imagery analytics where performed on Google Earth Engine(GEE) platform using java script. NDCI(normalized cloud index)= (sur_refl_b01 - sur_refl_b06)/(sur_refl_b01 + sur_refl_b06).
Imagery processing SDS cloud flag was used is a base for cloud mapping; LOW PASS process NDCI greather than -0.14; MID PASS process NDCI greather than 0; HIGH PASS processes red band surface reflectance greather than 0.6.
![Sahara_screenshotsGEE](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/3f663f55-6e9c-431a-ab44-6186f1465d53)
![Sahara_screenshotsGEE_low](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/8bc6452f-d41f-46dc-aed5-8cc52e129217)
![Sahara_screenshotsGEE_mid](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/91a29de0-e013-411e-87cb-d8410832e77e)
*fast grafic presentation: screenshoots from GEE (~)center is three-borders(Algeria,Mali,Niger); RGB(MODIS SR 10.jan.2015); WHITE = SDS cloud flag, BLUE = low_pass, YELLOW = mid_pass, RED = high_pass*



### Results:
![Cloudiness by DOY  2000-2023 average (N=8194)](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/cad0f5bb-5c60-4cf6-84f5-bde30f9b35f0)

![Average cloudiness of weeks__march2000-march2023](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/53bb48ee-16e1-4998-a72a-ef9f549f226b)

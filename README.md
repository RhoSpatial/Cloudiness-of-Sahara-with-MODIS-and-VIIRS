# Cloudiness-of-Sahara-with-MODIS

### Study area

![Stuady_are1](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/083c5186-a60a-4deb-b9fd-b9e28cefdeba)

### Clouds area retrieving method:
Imagery analytics where performed on Google Earth Engine(GEE) platform using java script.MODIS NDCI(normalized cloud index)= (sur_refl_b01 - sur_refl_b06)/(sur_refl_b01 + sur_refl_b06).
Imagery processing: SDS cloud flag was used as a base for cloud mapping; LOW PASS processes NDCI greather than -0.14; MID PASS processes NDCI greather than 0; HIGH PASS processes red band surface reflectance greather than 0.6.
![Sahara_screenshotsGEE](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/3f663f55-6e9c-431a-ab44-6186f1465d53)
*fast grafic presentation: screenshoots from GEE (~)center is at three borders(Algeria,Mali,Niger); RGB(MODISTerra SR 10.jan.2015); WHITE = SDS cloud flag, BLUE = low_pass, YELLOW = mid_pass, RED = high_pass*



### Results:
![Cloudiness by DOY  2000-23 average (N=8194; 97,5% of total) days MODISterra](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/9ebae7b5-f925-4c03-8827-055e507bd432)

![Cloudiness by DOY  2000-2023 average (N=8194)](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/cad0f5bb-5c60-4cf6-84f5-bde30f9b35f0)
![Yearly averages 2001-22 MODIS TERRA](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/9cdefc41-66d0-4c95-85c0-6049466da7c0)

![_MODIS Terra   2001-2022    (min, quartiles, max) ](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/bf7a9b0f-a736-4ec7-8f49-87c3b362b441)


#### MODIS Terra maps:
Number of cloudy days in one year in each pixel (20 years averages 2000-2020). This maps where exported from GEE and processed in SAGA-GIS; they don't exclude days with significant sensor 
failer(like px_count in study area), therefore the true values [days/year] are slightly higher. This maps were made before the study area was drawn.

![Sahara_red_high_pass](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/55510733-3b0d-41d7-888c-26abe4b94e42)

![Sahara_mid_pass](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/ceaa9eac-4a7e-4d24-a396-d5823b8429d4)

Averages on areas. Areas where obtained by reclasifiying, resempling and segmentation of SDS(Cloud flag) map.
![Sahara_high_mid_area](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/b3887bcf-1c2f-4014-9d8e-259d314c652d)

![Sahara_low_pass](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/b5e52e6a-9ebd-4675-acab-fba819ae810d)

![Sahara_SDS_pass](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/e46d9751-e8c9-40c7-8c4a-b7957d5dd019)

![Sahara_low_SDS_area](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/e81499cd-937f-4bb1-856d-dcbc3e07eab4)




![Sahara_DaysHighpass](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/a3f92e3a-f824-497b-85a0-16cbaf4bda6d)

![Sahara_DaysMIDpass](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/b9b376bf-ef11-4596-9dd3-d17ef10a065f)
![Sahara_cloudyDaysLowpass](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/deeebd27-9d23-40a1-a28f-0f39d7a3086c)

![Sahara_cloudyDaysSDS](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/db349cce-0f0e-44bb-bcd5-90adbb55891e)




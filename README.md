# Cloudiness-of-Sahara-with-MODIS

### Study area

![Stuady_are1](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/083c5186-a60a-4deb-b9fd-b9e28cefdeba)

### Clouds area retrieving method:
Imagery analytics where performed on Google Earth Engine(GEE) platform using java script.MODIS NDCI(normalized cloud index)= (sur_refl_b01 - sur_refl_b06)/(sur_refl_b01 + sur_refl_b06).
Imagery processing: SDS cloud flag was used as a base for cloud mapping; LOW PASS processes NDCI greather than -0.14; MID PASS processes NDCI greather than 0; HIGH PASS processes red band surface reflectance greather than 0.6.
![Sahara_screenshotsGEE](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/3f663f55-6e9c-431a-ab44-6186f1465d53)
*fast grafic presentation: screenshoots from GEE (~)center is at three borders(Algeria,Mali,Niger); RGB(MODISTerra SR 10.jan.2015); WHITE = SDS cloud flag, BLUE = low_pass, YELLOW = mid_pass, RED = high_pass*



### Results:
![Cloudiness by DOY  2000-2023 average (N=8194)](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/cad0f5bb-5c60-4cf6-84f5-bde30f9b35f0)

![Average cloudiness of weeks__march2000-march2023](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/53bb48ee-16e1-4998-a72a-ef9f549f226b)

#### MODIS Terra maps: number of cloudy days in one year in each pixel (20 years averages 2000-2020)

![Sahara_DaysHighpass](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/a3f92e3a-f824-497b-85a0-16cbaf4bda6d)

![Sahara_DaysMIDpass](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/b9b376bf-ef11-4596-9dd3-d17ef10a065f)
![Sahara_cloudyDaysLowpass](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/deeebd27-9d23-40a1-a28f-0f39d7a3086c)

![Sahara_cloudyDaysSDS](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/db349cce-0f0e-44bb-bcd5-90adbb55891e)




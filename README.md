# Cloudiness-of-Sahara-with-MODIS

´´´java

var MODIS_SR_coll = ee.ImageCollection('MODIS/061/MOD09GA')
       .filterDate('2002-07-04', '2003-07-04')
       .select(['state_1km','sur_refl_b01','sur_refl_b06'])
       .map(function(i){return i.clip(Sahara_study)});
       
´´´
### Study area

![Stuady_are1](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/083c5186-a60a-4deb-b9fd-b9e28cefdeba)

### Clouds area retrieving method:
Imagery analytics where performed on Google Earth Engine(GEE) platform using java script.MODIS NDCI(normalized cloud index)= (sur_refl_b01 - sur_refl_b06)/(sur_refl_b01 + sur_refl_b06).
Imagery processing: SDS cloud flag was used as a base for cloud mapping; LOW PASS processes NDCI greather than -0.14; MID PASS processes NDCI greather than 0; HIGH PASS processes red band surface reflectance greather than 0.6.
![Sahara_screenshotsGEE](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/3f663f55-6e9c-431a-ab44-6186f1465d53)
*fast grafic presentation: screenshoots from GEE (~)center is at three borders(Algeria,Mali,Niger); RGB(MODISTerra SR 10.jan.2015); WHITE = SDS cloud flag, BLUE = low_pass, YELLOW = mid_pass, RED = high_pass*

 Used MODIS bands:
 500m Surface Reflectance Band 1: (620-670 nm)
 500m Surface Reflectance Band 6: (1628-1652 nm)
1000m state_1km: Reflectance data state QA flags, SDS(Scientific Data Set), cloud state

### Results:
![Cloudiness by DOY  2000-23 average (N=8194; 97,5% of total) days MODIS_Terra](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/df769e65-0573-4120-aa57-9277b1354de5)

![_MODIS Terra   2001-2022    MID-pass  (min, quartiles, max) ](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/455d6205-1efd-4e65-b7d2-72067951d95d)

![Yearly averages 2001-22 MODIS TERRA  (Sahara)](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/8adb2b3a-faf2-4828-91ab-33b608409c41)

### Java script

´´´
var MODIS_SR_coll = ee.ImageCollection('MODIS/061/MOD09GA') //MODIS/061/MYD09GA
       .filterDate('2002-07-04', '2003-07-04')
       .select(['state_1km','sur_refl_b01','sur_refl_b06'])
       .map(function(i){return i.clip(Sahara_study)});
       
var low_pass_th = -0.14;
var mid_pass_th = 0;
var red_high_pass = 6000;

function Clouds_MOD09GA(i) {
    var date = i.date().format('YYYY-MM-dd');
    var year = i.date().get('year');
    var month = i.date().get('month');
    var week_Y = i.date().get('week');
    var day_Y = i.date().format('DDD');
    var C_flag = i.select('state_1km').bitwiseAnd(1 << 0).eq(1).rename('Cflag_state_1km');
    
    var NDCI = i.normalizedDifference(['sur_refl_b01', 'sur_refl_b06']).rename('NDCI_redswir1');
    var low_pass_NDCI = NDCI.gt(low_pass_th).and(C_flag.eq(1)).rename('NDCI_low_pass');
    var mid_pass_NDCI = NDCI.gt(mid_pass_th).and(C_flag.eq(1)).rename('NDCI_mid_pass');
    var high_pass_RED = mid_pass_NDCI.eq(1).and(i.select('sur_refl_b01').gt(red_high_pass)).rename('RED_high_pass');

  var low_pass_C = low_pass_NDCI.reduceRegion({
  reducer: ee.Reducer.mean().unweighted(),
  geometry: Sahara_study,
  scale: 500, 
  maxPixels: 1e9,}).values();
  
  var mid_pass_C = mid_pass_NDCI.reduceRegion({
  reducer: ee.Reducer.mean().unweighted(),
  geometry: Sahara_study,
  scale: 500, 
  maxPixels: 1e9,}).values();
  
  var high_pass_C = high_pass_RED.reduceRegion({
  reducer: ee.Reducer.mean().unweighted(),
  geometry: Sahara_study,
  scale: 500, 
  maxPixels: 1e9,}).values();
  
  var count_GA = i.select('sur_refl_b01').reduceRegion({
  reducer: ee.Reducer.count().unweighted(),
  geometry: Sahara_study,
  scale: 500, 
  maxPixels: 1e9,}).values();
  
  var row = ee.List([date, year, month, week_Y, day_Y, low_pass_C, mid_pass_C, high_pass_C, count_GA]).flatten();

return ee.Feature(null, {'rows1': row});
}

var MODIS_values = MODIS_SR_coll.map(Clouds_MOD09GA);
//print('fColl',MODIS_values);
//print('No. of days',MODIS_values.size());//toList(MODIS_values.size()));

Export.table.toDrive({
  collection: MODIS_values,
  description: 'SAHARA_clouds_00-03-01_23-03-01',
  folder: 'GEE_sahra',
  fileFormat: 'CSV',
  selectors: ['rows1']
});
´´´

#### MODIS Terra maps:
Number of cloudy days in one year in each pixel (20 years averages 2000-2020). This maps where exported from GEE and processed in SAGA-GIS; they don't exclude days with significant sensor 
failure(like px_count in study area), therefore the true values [days/year] are slightly higher. This maps were made before the study area was drawn.
High pass SDS_C_flag & red_band refl(gt 0.6)
![Sahara_red_high_pass](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/55510733-3b0d-41d7-888c-26abe4b94e42)

![Sahara_mid_pass](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/ceaa9eac-4a7e-4d24-a396-d5823b8429d4)

Averages on areas. Areas where obtained by reclasifiying, resempling and segmentation of SDS(Cloud flag) map.
![Sahara_high_mid_area](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/b3887bcf-1c2f-4014-9d8e-259d314c652d)

![Sahara_low_pass](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/b5e52e6a-9ebd-4675-acab-fba819ae810d)

![Sahara_SDS_pass](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/e46d9751-e8c9-40c7-8c4a-b7957d5dd019)

![Sahara_low_SDS_area](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/e81499cd-937f-4bb1-856d-dcbc3e07eab4)


#### Terra vs Agua
![Cloudiness by DOY  2002-23 average (N=7467; 97,35% of total) days MODIS_Aqua](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/e129d8a1-9ab8-4011-97c7-18b35e8e22a5)

![MODIS Aqua   2003-2022   MID-pass (min, quartiles, max) ](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/a472f5da-cc09-44c2-b055-de093087ce5a)

![Yearly averages 2003-22 MODIS AQUA (Sahara)](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/9e11150f-835a-4bef-b430-92729b695c12)




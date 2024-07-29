# Sahara-Cloudiness-with-MODIS-using-GEE

### Study area
~ 5 000 000 km2 (little less)

![Stuady_are1](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/083c5186-a60a-4deb-b9fd-b9e28cefdeba)

### Clouds area retrieving method:
Imagery analytics where performed on Google Earth Engine(GEE) platform using java script and MODIS Surface Reflectance product: MOD09GA version 6.1;<br/>
NDCI = Normalized Difference Cloud Index<br/>

NDCI = (sur_refl_b01 - sur_refl_b06)/(sur_refl_b01 + sur_refl_b06).<br/>

Imagery processing:<br/> 1 SDS(C_flag)State_1km Bitmask QA; cloud state bit was used as a mask for cloud mapping;<br/> 2 LOW PASS includes pixels with NDCI greather than -0.14<br/> 3 MID PASS processes NDCI greather than 0<br/> 4 HIGH PASS processes red band(b01) surface reflectance greather than 0.6.

![Sahara_screenshotsGEE](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/3f663f55-6e9c-431a-ab44-6186f1465d53)
<sub>*fast grafic presentation: screenshoots from GEE (~)center is at three borders(Algeria,Mali,Niger); RGB(MODISTerra SR 10.jan.2015);</sub><br/>

WHITE = QA(SDS cloud flag)<br/> BLUE = low_pass<br/> YELLOW = mid_pass<br/> RED = high_pass

 <sub>Used MODIS bands:<br/>
 500m Surface Reflectance Band 1: (620-670 nm); red<br/>
 500m Surface Reflectance Band 6: (1628-1652 nm); SWIR I<br/>
1000m state_1km: Reflectance data state QA flags, SDS(Scientific Data Set), cloud state</sub>

### Results:
Average cloudiness of study area is ~ 8 %.<br/>
![Cloudiness by DOY  2000-23 average (N=8194; 97,5% of total) days MODIS_Terra](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/df769e65-0573-4120-aa57-9277b1354de5)

![_MODIS Terra   2001-2022    MID-pass  (min, quartiles, max) ](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/455d6205-1efd-4e65-b7d2-72067951d95d)

![Yearly averages 2001-22 MODIS TERRA  (Sahara)](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/8adb2b3a-faf2-4828-91ab-33b608409c41)

### Main Java script for retrieving data
Script is optimized to process and export results from large data input where output preserves input bands resolution.There is no GUI, just table export.
```js
var MODIS_SR_coll = ee.ImageCollection('MODIS/061/MOD09GA') //MODIS/061/MYD09GA
       .filterDate('2000-03-01', '2000-04-01')
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

Export.table.toDrive({
  collection: MODIS_SR_coll.map(Clouds_MOD09GA),
  description: 'description',
  folder: 'GEE_sahra',
  fileFormat: 'CSV',
  selectors: ['rows1']
});
```
<br/>


`MAX cloudiness distribution`
![No  of days with LOW_pass cloudiness higher than 18%  N= 838](https://github.com/user-attachments/assets/41d91435-3217-4858-a88e-43024bb389c7)


`MIN cloudiness distribution`
![LOW_pass  cloudiness](https://github.com/user-attachments/assets/d9c40fc5-a683-4113-92dd-dc90d370b7c9)


<sub>Study area had zero cloud cover on three days: 20.dec 2006, 7.feb 2010 and 11.mar 2018. <\sub>

![ZeroCloud_screen_MIN_WorldView](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/a7232d35-fd45-4024-b00b-c51fe75b9dd6)


`filtering based on counting valid pixels in study area`

Analysed imagery has time interval 1.march 2000-27.feb 2023, that is total 8400 days(68 days had no data on study area; additional 138 days where removed later
in Google Sheets, where days with `count_GA` higher than 18.545.000 px are considered valid.

![Px_count_chart](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/f58326a3-c327-49ed-811f-1206f5cf1cea)

![Sahar_screenFAIL17_21_WorldV](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/98ce3966-3af5-464f-911c-b1388a9dbe51)

`No. of daily imagery used in analytics`
![Valid Imagery per Year](https://github.com/user-attachments/assets/3dc2c6b1-e240-4472-abc9-580ed6de301f)



#### MODIS Terra maps:
Number of cloudy days in one year in each pixel (20 years averages 2000-2020). This maps where exported from GEE and processed in SAGA-GIS; they don't exclude days with significant sensor 
failure(like px_count in study area), therefore the true values [days/year] are slightly higher(export from GEE was sum). This maps were made before the study area was drawn.<br/>

High pass <sub>QA-C_flag & red_band refl gt(0.6)</sub>
![Sahara_red_high_pass](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/55510733-3b0d-41d7-888c-26abe4b94e42)

MID pass <sub>QA-C_flag & NDCI gt(0)</sub>
![Sahara_mid_pass](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/ceaa9eac-4a7e-4d24-a396-d5823b8429d4)

HIGH pass | MID pass <sub>Avg no. of days on areas...</sub>
<sub>Areas where generated by reclasifiying, resampling and segmentation of QA_SDS(Cloud flag) map in SAGA-GIS.</sub>

![Sahara_high_mid_area](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/b3887bcf-1c2f-4014-9d8e-259d314c652d)

LOW pass <sub>QA_C_flag & NDCI gt(-0.14)</sub>
![Sahara_low_pass](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/b5e52e6a-9ebd-4675-acab-fba819ae810d)

QA-C_flag <sub>state_1km Bitmask (bit 0 = 1)</sub>
![Sahara_SDS_pass](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/e46d9751-e8c9-40c7-8c4a-b7957d5dd019)

LOW pass | QA(C_flag) <sub>Avg no. of days on areas...</sub>
![Sahara_low_SDS_area](https://github.com/RhoSpatial/Cloudiness-of-Sahara-with-MODIS-and-VIIRS/assets/111765142/e81499cd-937f-4bb1-856d-dcbc3e07eab4)


### Visualization Java script

```js
var MODIS_SR_coll = ee.ImageCollection('MODIS/061/MOD09GA')
       .filterDate('2000-03-01', '2000-03-16')
       .select(['state_1km','sur_refl_b01','sur_refl_b06'])
       .map(function(i){return i.clip(Sahara_study)});

var low_pass_th = -0.14;
var mid_pass_th = 0;
var red_high_pass = 6000;

function Clouds_MOD09GA(i) {
    var C_flag = i.select('state_1km').bitwiseAnd(1 << 0).eq(1).rename('Cflag_state_1km');
    var NDCI = i.normalizedDifference(['sur_refl_b01', 'sur_refl_b06']).rename('NDCI_redswir1');
    var low_pass_NDCI = NDCI.gt(low_pass_th).and(C_flag.eq(1)).rename('NDCI_low_pass');
    var mid_pass_NDCI = NDCI.gt(mid_pass_th).and(C_flag.eq(1)).rename('NDCI_mid_pass');
    var high_pass_RED = mid_pass_NDCI.eq(1).and(i.select('sur_refl_b01').gt(red_high_pass)).rename('RED_high_pass');
  
  return ee.Image([C_flag, low_pass_NDCI, mid_pass_NDCI, high_pass_RED]).copyProperties(i,['system:time_start']);
}

var OUTPUT_images = MODIS_SR_coll.map(Clouds_MOD09GA);

var OUTPUT_List = ee.Image(OUTPUT_images.toList(OUTPUT_images.size()));
print('OUTPUT_List', OUTPUT_List);

var listed_Day = 0;

var OUTPUT_Day = ee.Image(OUTPUT_images.toList(OUTPUT_images.size()).get(listed_Day));
print('listed Day :'+listed_Day, ee.Date((OUTPUT_Day.get('system:time_start'))));
print('Study_area_m2', Sahara_study.area());

var Cmask_chart = ui.Chart.image.seriesByRegion(OUTPUT_images, Sahara_study, 
      ee.Reducer.mean(), 'Cflag_state_1km', 1000, 'system:time_start','label')
        .setOptions({
          title: 'Cloudines MODIS_sr Cmask',
          vAxis: {title: 'band: state_1km Bitmask'},
          pointSize: 2,
          colors: ['E37D05']
        });
print(Cmask_chart);

var NDCI_low_chart = ui.Chart.image.seriesByRegion(OUTPUT_images, Sahara_study, 
      ee.Reducer.mean(),'NDCI_low_pass', 500, 'system:time_start','label')
        .setOptions({
          title: 'Cloudines MODIS_sr NDCI_low_pass',
          vAxis: {title: 'Cmask and NDCI_gt'+low_pass_th},
          pointSize: 2,
          colors: ['#163bff']});
print(NDCI_low_chart);

var NDCI_mid_chart = ui.Chart.image.seriesByRegion(OUTPUT_images, Sahara_study, 
      ee.Reducer.mean(),'NDCI_mid_pass', 500, 'system:time_start','label')
        .setOptions({
          title: 'Cloudines MODIS_sr NDCI_mid_pass',
          vAxis: {title: 'Cmask and NDCI_gt'+mid_pass_th},
          pointSize: 2,
          colors: ['#fff23b']});
print(NDCI_mid_chart);

var RED_high_chart = ui.Chart.image.seriesByRegion(OUTPUT_images, Sahara_study, 
      ee.Reducer.mean(), 'RED_high_pass', 500, 'system:time_start','label')
        .setOptions({
          title: 'Cloudines MODIS_sr RED high pass',
          vAxis: {title: 'Cmask and RED_sr_gt ('+red_high_pass},
          pointSize: 2,
          colors: ['#ff5371']});
print(RED_high_chart);

var px_count_chart = ui.Chart.image.seriesByRegion(OUTPUT_images, Sahara_study, 
      ee.Reducer.count(), 'RED_high_pass', 500, 'system:time_start','label')
        .setOptions({
          title: 'No. of valid pixels(500m res) in study area',
          vAxis: {title: 'valid pixel px_count'},
          lineWidth: 1,
          pointSize: 2,
          colors: ['#ff5371']
        });
print(px_count_chart);

var MODIS_SR_day = ee.ImageCollection('MODIS/061/MOD09GA')
                                    .filterDate(ee.Date(OUTPUT_Day.get('system:time_start')));
                                    
Map.addLayer(MODIS_SR_day,{'bands':['sur_refl_b01','sur_refl_b04','sur_refl_b03'],'min':100,'max':8000},'rgb');
Map.addLayer(OUTPUT_Day.select('Cflag_state_1km').selfMask(),{palette:'#ffffff'},'C_flag_cloud_state' ,false);
Map.addLayer(OUTPUT_Day.select('NDCI_low_pass').selfMask(),{palette:'#163bff'},'NDCI_low_pass');
Map.addLayer(OUTPUT_Day.select('NDCI_mid_pass').selfMask(),{palette:'#fff23b'},'NDCI_mid_pass',false);
Map.addLayer(OUTPUT_Day.select('RED_high_pass').selfMask(),{palette:'#ff5371'},'RED_high_pass');
```

### Discussion

![No  of _valid days_ MODIS Terra Sahara](https://github.com/user-attachments/assets/b8af848b-367a-4283-8e8f-b820de280f87)


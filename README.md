# Analysis of glaciers and glacial lakes in SAGA GIS
***
## Introduction
High Mountain Asia contains, apart from the polar regions, the largest glacierized areas and is therefore often called the "Third Pole" (Rowan et al. 2018, Zhang et al. 2021).

Due to the size of the area and spatial heterogeneity within, different trends and patterns were observed, ranging from massive glacier decline in Nepal to an increase in glacier area in the Karakoram (coining the term "Karakoram Anomaly") (Nie et al. 2021, Bolch et al. 2012).
While some areas were and are studied extensively, other regions were less in the focus of scientific work, leaving broad knowledge gaps that need to be investigated. 

This study aims to analyze the behavior of glaciers in the Ladakh Range (India) and set it also in relation to the evolution and development of glacial lakes. 
A change detection was conducted using two Landsat scenes, to compare glacier and glacial lake area from 2000 (17th September) and 2019 (10th September). 
Both scenes are from September to have a minimum of snow cover and to ensure comparability. 

The analysis was done using SAGA GIS. The used data was acquired from USGS Earth Explorer (https://earthexplorer.usgs.gov/) and OpenTopography (https://opentopography.org).

***
## Workflow
<figure>
  <img src="https://github.com/lenajaeger9/DigitalImageAnalysis_FinalTask/blob/main/figures/workflow.png" alt="Workflow" width="900">
</figure>

Figure 1 shows the workflow of the conducted analysis.
Since glaciers and glacial lakes are part of the hydrological system, a watershed was delineated as study area. 
Choosing the Upslope Area to be interactive, one can manually select a point in the map. 
Depending on this point the area can consequently encompass a larger watershed or on the contrary be a minor sub-watershed.
Further, one important step is to always make sure to use the ```Coordinate Transformation``` tool to match the projections, otherwise it is a very common source of errors. 

Different indices were assessed:

| Index                     |               Formula               |
|---------------------------|:-----------------------------------:|
| NDWI (McFeeters 1996)     |   ((GREEN - NIR) / (GREEN + NIR)    |
| MNDWI/ NDSI               | ((GREEN - SWIR1) / (GREEN + SWIR1)) |
| NDGI                      |   ((GREEN - RED) / (GREEN + RED))   |
| NDWI (Huggel et al. 2002) |    ((NIR - BLUE) / (NIR + BLUE))    |
| NDVI                      |     ((NIR - RED) / (NIR + RED))     |


Out of the indices, the MNDWI delivered the best results to delineate glaciers, while McFeeters' NDWI could nicely differentiate water bodies.
For the water mask a threshold of 0.3 was chosen, both for glacier and water area.

### Side Note
* It was first tried to conduct a supervised classification, extracting training data from RGB and Idice visualizations, with four classes: glacier (ice/snow), water, other and mountain shadow.  However, also due to the lack of ground truth data, the result was not satisfactory, so water and glacier areas were extracted based on the NDWI and MNDWI.
* During the analysis it happened, that two Landsat scenes were cropped to the AOI but had different grids. Since following steps like the confusion matrix can only be conducted for the same grids, it was necessary to adjust the two grids. In SAGA this can easily be done using the ````Resampling```` tool. 

*** 
## Results
The used methodology was efficient, delineating the glacier and glacial lake area, as it can be seen in the figures below. 
Shadowed areas however were a major challenge and is the biggest source of error. 

| <img src="https://github.com/lenajaeger9/DigitalImageAnalysis_FinalTask/blob/main/figures/glacier_mask2000.png" alt="Glacier mask 2000" width="400"/> | <img src="https://github.com/lenajaeger9/DigitalImageAnalysis_FinalTask/blob/main/figures/glacier_mask2019.png" alt="Glacier mask 2019" width="400"/> |
| -- | --- |
| Image 1 | Image 2 |

The results showed a clear decline in glacier area. Looking at the confusion matrix, 16337 pixels turned from "glacier" to "unclassified", 
which means that 14.73 km² of glacier area (Landsat pixel size is 30m) deglaciated in the time span of 19 years. 

| <img src="https://github.com/lenajaeger9/DigitalImageAnalysis_FinalTask/blob/main/figures/ConfusionMatrix.png" alt="Glacier mask 2000" width="400"/> | <img src="https://github.com/lenajaeger9/DigitalImageAnalysis_FinalTask/blob/main/figures/Confusion%20Matrix%202000%20-%202019.png" width="400"/> |
| -- | --- |
| Image 1 | Image 2 |


<figure>
  <img src="https://github.com/lenajaeger9/DigitalImageAnalysis_FinalTask/blob/main/figures/ConfusionMatrix.png" alt="Workflow" width="450">
</figure>

-->  Confusion matrix 
--> threshold 0.3 ()

***
## Discussion 


***
## Literature
* Bolch, T.; Kulkarni, A.; Kääb, A.; Huggel, C.; Paul, F.; Cogley, J.G.; Frey, H.; Kargel, J.S.; Fujita, K.; Scheel, M.; Bajracharya, S.; Stoffel, M. (2012): The State and Fate of Himalayan Glaciers. Science, 336(6079), pp. 310-314.
* Li, J.Y.; Ding, J.Y.; Shangguan, D.H.; Wang, R.J. (2019): Regional differences in global glacier retreat from 1980 to 2015. Advances in Climate Change Research, 10(4), pp. 203-213. DOI: https://doi.org/10.1016/j.accre.2020.03.003.
* Nie, Y.; Pritchard, H.D.; Liu, Q.; Hennig, T.; Wang, W.; Wang, X.; Liu, S.; Nepal, S.; Samyn, D.; Hewitt, K.; Chen, X. (2021): Glacial change and hydrological implications in the Himalaya and Karakoram. Nature Reviews Earth & Environment, 2(2), pp. 91-106. DOI: https://doi.org/10.1038/s43017-020-00124-w.
* Rowan, A.V.; Quincey, D.J.; Gibson, M.J.; Glasser, N.F.; Westoby, M.J.; Irvine-Fynn, T.D.L.; Porter, P.R.; Hambrey, M.J. (2018): The sustainability of water resources in High Mountain Asia in the context of recent and future glacier change. Geological Society, Special Publications, 462(1), London, pp. 189-204.
* Zheng, G.; Keith Allen, S.; Bao, A.; Ballesteros- Cánovas, J.A.; Huss, M.; Zhang, G.; Li, J.; Yuan, Y.; Jiang, L.; Yu, T.; Chen, W.; Stoffel, M. (2021): Increasing risk of glacial lake outburst floods from future Third Pole deglaciation. Nature Climate Change, 11(5), pp. 411-417. 
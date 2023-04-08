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
For the masks a threshold of 0.3 was chosen, both for glacier and water area.

### Side Note
* It was first tried to conduct a supervised classification, extracting training data from RGB and Idice visualizations, with four classes: glacier (ice/snow), water, other and mountain shadow.  However, also due to the lack of ground truth data, the result was not satisfactory, so water and glacier areas were extracted based on the NDWI and MNDWI.
* During the analysis it happened, that two Landsat scenes were cropped to the AOI but had different grids. Since following steps like the confusion matrix can only be conducted for the same grids, it was necessary to adjust the two grids. In SAGA this can easily be done using the ````Resampling```` tool. 

*** 
## Results

The used methodology was efficient, delineating the glacier and glacial lake area, as it can be seen in the figures below. 
Shadowed areas however were a major challenge and is the biggest source of error. 

| <img src="https://github.com/lenajaeger9/DigitalImageAnalysis_FinalTask/blob/main/figures/glacier_mask2000.png" alt="Glacier mask 2000" width="400"/> | <img src="https://github.com/lenajaeger9/DigitalImageAnalysis_FinalTask/blob/main/figures/glacier_mask2019.png" alt="Glacier mask 2019" width="400"/> |
| -- | --- |
| Glacier mask 2000 | Glacier mask 2019 |

The results showed a clear decline in glacier area. 
19915 pixels turned from glacier to unclassified, which is an area of 17.9235 km². 3815 pixels (3.4335 km²) showed the contrary development, being "unclassified" in 2000 and classified as "glacier" in 2019.

<figure>
  <img src="https://github.com/lenajaeger9/DigitalImageAnalysis_FinalTask/blob/main/figures/3D_glacier_outline2.png" alt="3D glacier outlines" width="700">
</figure>

For a better illustration, the figure above shows the outlines in a 3D visualization, the dark blue outline being from 2000, while the cyan colored lines represent 2019. 
As background, an RGB composite of the 2019 scene was chosen, to assess the accuracy of the outlines. It can be seen, that the outlines of 2019 are quite well matching the snow/ ice covered areas. 
Yet, in the valley there are two lakes that got classified as glaciers, showing that the mask should be revised. 
At the same time, it is nicely shown that the glacier tongue on the three glaciers retreated over time, along with different snow or ice fields. 

Zooming in to get a better look on single glaciers, reducing the extent of the AOI, the results remain clear. 
Looking at the confusion matrix, 16337 pixels turned from "glacier" to "unclassified", 
which means that 14.73 km² of glacier area (Landsat pixel size is 30m) deglaciated in the time span of 19 years. 
This is also illustrated below, where the orange masked area represents the area that turned from "glacier" to "unclassified". 

<figure>
  <img src="https://github.com/lenajaeger9/DigitalImageAnalysis_FinalTask/blob/main/figures/ConfusionMatrix.png" alt="Confusion Matrix" width="500">
</figure>

<figure>
  <img src="https://github.com/lenajaeger9/DigitalImageAnalysis_FinalTask/blob/main/figures/Confusion%20Matrix%202000%20-%202019.png" alt="Confusion Matrix" width="450">
</figure>

While the glacier areas clearly decreased, glacial lakes in turn increased in area, although not as strong as glaciers. 
The number of pixels that turned from "Unclassified" to "water" is 166, so 0.1494 km². 
This area seems small, especially compared to 14.73 km², however considering that it concerns (generally small) lakes, it is quite a big change. 
To clarify this, the 3D image below shows the lake outlines. 

<figure>
  <img src="https://github.com/lenajaeger9/DigitalImageAnalysis_FinalTask/blob/main/figures/3D_outlines.png" alt="3D glacial lake outlines" width="550">
</figure>

Here, it can be seen that beside the fact that existing lakes increased in size, also a new lake evolved. 
The lake outlines of 2000 are colored green, the ones from 2019 are displayed in fuchsia. 

In summary it can be said that for the study area, the area of glaciers overall clearly decreased, while the area (and number) of glacial lakes increased. 

[3D-demo.mp4](..%2FDigital%20Image%20Analysis%2Ffigures%2F3D-demo.mp4)

***
## Discussion 
As the current analysis only compared two Landsat scenes, the insights and findings are on the one hand very limited and do not allow to make hasty conclusions.
On the other hand it is alarming and points out that more research is urgently needed.

Another aspect that was already mentioned in the results, is the misclassification of water areas as glaciers.
One idea to improve this error would be to not only set a threshold, but rather a value range (from 0.3 to 0.6 or similar) when calculating the glacier mask based on the MNDWI.
Doing so, it should be first checked in which value range the water bodies lie and if they differentiate from glacier areas, to see wether it makes sense and is feasible. 

### Working with SAGA GIS
Lastly, SAGA GIS is a free open source software, with valuable tools to do geographic analysis. Constant work is done to improve the software. 
If the interface seems not easy to handle, with the ```Find and Run Tool```, one does not need to scroll through all the tabs to find the right tool for the analysis.
The fast and easy way to display a 3D visualization is also an advantage.
Nonetheless, the Print Layout is very plain and simplistic.
Especially compared to QGIS, the possibilities of creating nice and maybe eye-catching maps are very limited with SAGA.
Yet, as the main aim of an analysis like the present is to conduct a methodological sound analysis with meaningful results, this is rather a small issue. 
It could be also considered switching to other software for visualization if needed. 

***
## Literature
* Bolch, T.; Kulkarni, A.; Kääb, A.; Huggel, C.; Paul, F.; Cogley, J.G.; Frey, H.; Kargel, J.S.; Fujita, K.; Scheel, M.; Bajracharya, S.; Stoffel, M. (2012): The State and Fate of Himalayan Glaciers. Science, 336(6079), pp. 310-314.
* Li, J.Y.; Ding, J.Y.; Shangguan, D.H.; Wang, R.J. (2019): Regional differences in global glacier retreat from 1980 to 2015. Advances in Climate Change Research, 10(4), pp. 203-213. DOI: https://doi.org/10.1016/j.accre.2020.03.003.
* Nie, Y.; Pritchard, H.D.; Liu, Q.; Hennig, T.; Wang, W.; Wang, X.; Liu, S.; Nepal, S.; Samyn, D.; Hewitt, K.; Chen, X. (2021): Glacial change and hydrological implications in the Himalaya and Karakoram. Nature Reviews Earth & Environment, 2(2), pp. 91-106. DOI: https://doi.org/10.1038/s43017-020-00124-w.
* Rowan, A.V.; Quincey, D.J.; Gibson, M.J.; Glasser, N.F.; Westoby, M.J.; Irvine-Fynn, T.D.L.; Porter, P.R.; Hambrey, M.J. (2018): The sustainability of water resources in High Mountain Asia in the context of recent and future glacier change. Geological Society, Special Publications, 462(1), London, pp. 189-204.
* Zheng, G.; Keith Allen, S.; Bao, A.; Ballesteros- Cánovas, J.A.; Huss, M.; Zhang, G.; Li, J.; Yuan, Y.; Jiang, L.; Yu, T.; Chen, W.; Stoffel, M. (2021): Increasing risk of glacial lake outburst floods from future Third Pole deglaciation. Nature Climate Change, 11(5), pp. 411-417. 
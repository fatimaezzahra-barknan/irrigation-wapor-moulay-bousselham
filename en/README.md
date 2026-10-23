🇬🇧 English | [🇫🇷 Français](README.fr.md)

# Crop water use and irrigation efficiency — Moulay Bousselham (Loukkos, Morocco)

Satellite analysis of crop water use in a coastal irrigated area, based on **FAO WaPOR v3** data at 20 m resolution (2018-2025).

**Research question.** Do crops receive enough water, too much, or too little? Where, and at what time of the season?

## Conclusion in three lines

1. The open field is split between an **irrigated core** (≈ 55 %) and **rainfed winter crops** (≈ 43 %); differences in water use arise **from May to August**, on irrigated fields only.
2. During the 2021-2023 drought, **irrigation compensated** for the rainfall deficit (2021-22: rainfall halved, irrigation + 38 %), with **stable water productivity** (4.2 to 4.5 kg/m³), slightly lower in dry years.
3. Among irrigated fields, 45 % are **well supplied**, 36 % **frugal and efficient** and 19 % **make poor use of water**, mostly along the lagoon; uniformity among irrigated fields improved (CV from 39 % to ≈ 20 %).

## Key figures

| | |
|---|---|
| ![Cluster map](figures/en/figure1_cluster_map.png) | **Figure 1.** Two-level K-means classification: cropping systems, then water management of irrigated fields. |
| ![Seasonal curve](figures/en/figure2_seasonal_curve.png) | **Figure 2.** Water use per dekad, from September to August, for each season (dry seasons in bold). |
| ![Water productivity](figures/en/figure3_water_productivity.png) | **Figure 3.** Water productivity per season (kg of biomass per m³ consumed). |

## Data

| Source | Variables | Resolution |
|---|---|---|
| FAO WaPOR v3 (`wapordl` package) | AETI (actual evapotranspiration), NPP (biomass) | 20 m, dekadal |
| FAO WaPOR v3 | RET (reference evapotranspiration), precipitation | coarse, dekadal |
| Esri / Impact Observatory, *10m Annual Land Use Land Cover* (Planetary Computer) | Land cover | 10 m, annual (2018-2023) |
| Manual digitisation (geojson.io) | 15 greenhouse blocks | — |

## Method

1. **Preparation**: daily means converted to dekad totals, RET and rainfall resampled onto the 20 m grid, "open field" (Esri "crops", excluding greenhouses) and "greenhouse" masks. WaPOR LCC no longer exists in v3 (error 404).
2. **Analysis by cropping season** (September → August), 7 complete seasons from 2018-19 to 2024-25.
3. **Four indicators**:
   - relative ET (AETI ÷ RET), compared with the best 5 % of pixels in the same dekad, since no field reaches the 0.7 threshold in summer;
   - irrigation supply = AETI − 0.8 × rainfall (effective rainfall), with no negative values;
   - water productivity = dry biomass (NPP × 22.222) ÷ AETI;
   - uniformity = coefficient of variation of AETI between pixels.
4. **Two-level K-means** on standardised, non-redundant variables; number of clusters validated with the elbow method and the silhouette coefficient (0.52 at level 1, 0.43 at level 2).

## Limitations

- Coarse-resolution RET and rainfall; RET probably overestimated near the coast.
- AETI underestimated under greenhouses: greenhouses excluded from the indicators.
- WaPOR biomass is total biomass (roots included): absolute productivity values should be read with caution.
- No Esri map for 2024-2025 (2023 map reused).
- Study area delineated visually, without the official boundaries of the irrigation scheme.
- Estimated irrigation is water consumed by crops, not water withdrawn.

## Repository contents

| File | Contents |
|---|---|
| `Irrigation_Gharb_WaPOR_EN.ipynb` | Notebook in English |
| `Irrigation_Gharb_WaPOR_FR.ipynb` | Notebook in French |
| `data/` | Study area outline and digitised greenhouses (GeoJSON) |
| `figures/en/`, `figures/fr/` | Key figures in English and French |

Variable names in the code are in French (e.g. `serres` = greenhouses, `pluie` = rainfall, `campagne` = cropping season).

## Reproducing the analysis

1. Open `Irrigation_Gharb_WaPOR_EN.ipynb` in Google Colab.
2. Copy the two files from `data/` to `My Drive/Projet_WaPOR/`.
3. `Runtime → Run all`. The first download takes about 2 h; the files are then reused.

## Sources

- FAO (2024). *WaPOR — Water Productivity through Open access of Remotely sensed derived data*, version 3. https://data.apps.fao.org/wapor
- Karra, K. et al. (2021). *Global land use/land cover with Sentinel-2 and deep learning*. Impact Observatory, Microsoft and Esri data, CC BY 4.0 licence. https://planetarycomputer.microsoft.com/dataset/io-lulc-annual-v02

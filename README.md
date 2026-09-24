# Ecological Dataset Mapping: Sources, Formats & Gaps

Bioregional finance / AI-agent-relevant catalog of publicly available ecological datasets. 18 datasets across watershed health, biodiversity, land use, carbon flux, and water quality.

**Legend:** 🤖 = machine-readable without preprocessing (clean API/GeoJSON/CSV/Parquet, no manual cleanup needed)

---

## Watershed Health

### 1. USGS National Water Information System (NWIS) 🤖
- **URL:** https://waterservices.usgs.gov/
- **License:** U.S. Government Work (public domain)
- **Coverage:** United States (streamgages, groundwater wells)
- **Temporal resolution:** Real-time (15-min) to historical daily/annual
- **Format:** REST API — JSON, WaterML (XML), RDB (tab-delimited)
- **Gaps:** US-only; gage density much lower in rural/western basins; some parameters (sediment, nutrients) have sparse station coverage.

### 2. EPA Water Quality Portal (WQP) 🤖
- **URL:** https://www.waterqualitydata.us/
- **License:** Public domain (US federal); some state-contributed data carries its own terms
- **Coverage:** United States (aggregates USGS, EPA STORET, USDA)
- **Temporal resolution:** Varies by station, back to early 1900s in places
- **Format:** REST API — CSV, JSON, WaterML
- **Gaps:** Data quality/methodology varies by contributing agency; inconsistent metadata across sources requires normalization even though the API itself is clean.

### 3. HydroSHEDS / HydroATLAS
- **URL:** https://www.hydrosheds.org/
- **License:** Free for non-commercial and most commercial use, per HydroSHEDS license terms
- **Coverage:** Global (derived from SRTM elevation data)
- **Temporal resolution:** Static (single snapshot, not time series)
- **Format:** Shapefile, GeoTIFF, GeoPackage
- **Gaps:** Static — no temporal change detection; resolution (~15 arc-sec / ~500m) too coarse for small watershed/parcel-level bioregional work.

### 4. NOAA National Water Model / NWM Reanalysis 🤖
- **URL:** https://water.noaa.gov/about/nwm
- **License:** Public domain (NOAA)
- **Coverage:** Continental United States
- **Temporal resolution:** Hourly forecasts; historical reanalysis back to 1979
- **Format:** NetCDF via NOMADS/AWS Open Data (s3://noaa-nwm-pds)
- **Gaps:** Modeled, not observed, streamflow — accuracy varies significantly by basin; US-only.

---

## Biodiversity

### 5. GBIF (Global Biodiversity Information Facility) 🤖
- **URL:** https://www.gbif.org/ (API: https://api.gbif.org/v1/)
- **License:** Mixed — CC0, CC BY, or CC BY-NC set per dataset/record (majority CC BY or CC0)
- **Coverage:** Global, 2.5B+ species occurrence records
- **Temporal resolution:** Continuous ingestion; monthly snapshots on AWS Open Data
- **Format:** REST API (JSON), Darwin Core Archive, Parquet snapshots
- **Gaps:** Strong sampling bias toward North America/Europe and toward well-studied taxa (birds, plants); citizen-science records (iNaturalist) dominate recent years and skew toward accessible/populated areas.

### 6. iNaturalist Open Data / GBIF-mediated feed 🤖
- **URL:** https://www.inaturalist.org/pages/developers ; bulk exports at https://registry.opendata.aws/inaturalist-open-data/
- **License:** Mixed CC licenses (CC0, CC BY, CC BY-NC) set per observation
- **Coverage:** Global, citizen-science observations
- **Temporal resolution:** Continuous, near-real-time
- **Format:** REST API (JSON); bulk CSV/Parquet on AWS
- **Gaps:** Observer-effort bias (urban/accessible areas over-represented); identification quality varies by "research grade" vs. casual observations.

### 7. eBird / EOD (eBird Observation Dataset) 🤖
- **URL:** https://ebird.org/data/download
- **License:** Free for non-commercial use under Cornell Lab's Terms of Use; commercial use requires permission
- **Coverage:** Global, strongest in Americas/Europe
- **Temporal resolution:** Checklist-level, near-real-time submissions since 2002
- **Format:** Bulk tab-delimited text files, API (JSON)
- **Gaps:** Birds only; requires a data-access request/agreement for full EOD download (not fully open self-serve); effort bias toward birdwatching hotspots.

### 8. IUCN Red List Spatial Data
- **URL:** https://www.iucnredlist.org/resources/spatial-data-download
- **License:** Free for non-commercial use; commercial use requires a license agreement
- **Coverage:** Global species range polygons
- **Temporal resolution:** Periodic updates (assessments run on multi-year cycles per taxon)
- **Format:** Shapefile/Geodatabase (requires GIS preprocessing)
- **Gaps:** Range maps are expert-drawn polygons, not observation points — coarse for local bioregional use; download requires manual request approval, not a live API.

---

## Land Use

### 9. ESA WorldCover 🤖
- **URL:** https://esa-worldcover.org/en ; https://registry.opendata.aws/esa-worldcover/
- **License:** CC BY 4.0
- **Coverage:** Global, 10m resolution
- **Temporal resolution:** Two epochs currently (2020, 2021); not annual
- **Format:** Cloud-Optimized GeoTIFF (COG) on AWS S3, plus WMS/API viewer
- **Gaps:** Only two time slices so far — limited for change-over-time analysis; 11-class land cover scheme is coarse for agricultural/bioregional sub-typing.

### 10. USDA Cropland Data Layer (CDL) 🤖
- **URL:** https://nassgeodata.gmu.edu/CropScape/
- **License:** Public domain (US federal)
- **Coverage:** Continental United States
- **Temporal resolution:** Annual, since ~1997 (full coverage since 2008)
- **Format:** GeoTIFF; REST API (CropScape/CropStat)
- **Gaps:** US-only; classification accuracy varies by crop type and region; not designed for non-agricultural land-use nuance (informal/urban ecological land use).

### 11. Global Forest Watch / Hansen Global Forest Change 🤖
- **URL:** https://www.globalforestwatch.org/ ; https://data.globalforestwatch.org/
- **License:** CC BY 4.0
- **Coverage:** Global, 30m resolution tree cover/loss
- **Temporal resolution:** Annual updates since 2000
- **Format:** REST API, GeoTIFF, vector tiles
- **Gaps:** "Tree cover loss" conflates natural disturbance, plantation harvest cycles, and deforestation — needs supplementary classification to distinguish drivers.

### 12. Global Human Settlement Layer (GHSL)
- **URL:** https://ghsl.jrc.ec.europa.eu/
- **License:** CC BY 4.0
- **Coverage:** Global, built-up area/population grids, resolutions from 10m to 1km
- **Temporal resolution:** Multi-decadal epochs (1975–2030 in ~5-year steps)
- **Format:** GeoTIFF (large raster preprocessing typically needed to clip/reproject)
- **Gaps:** Epoch-based rather than continuous; large global rasters require significant preprocessing/tiling for local use.

---

## Carbon Flux

### 13. FLUXNET (2015 release + FLUXNET Shuttle)
- **URL:** https://fluxnet.org/data/fluxnet2015-dataset/
- **License:** CC BY 4.0 (varies slightly by legacy site/regional network)
- **Coverage:** Global, ~1000+ eddy-covariance tower sites (point measurements, sparse in Africa/Asia/South America)
- **Temporal resolution:** Half-hourly to annual, multi-decadal at long-running sites
- **Format:** CSV per site (bulk download); newer "FLUXNET Shuttle" system moving toward continuously updated open access
- **Gaps:** Point-source data (200m–1km footprint) — not a continuous surface; strong geographic bias toward North America/Europe; gaps require gap-filling models (ONEFlux) before use.

### 14. NASA/ORNL DAAC — MODIS Net Primary Productivity (MOD17) 🤖
- **URL:** https://daac.ornl.gov/cgi-bin/dsviewer.pl?ds_id=1858
- **License:** Public domain (NASA)
- **Coverage:** Global, 500m resolution
- **Temporal resolution:** 8-day and annual composites, since 2000
- **Format:** HDF/GeoTIFF; API access via AppEEARS
- **Gaps:** Modeled from satellite reflectance + meteorology, not direct flux measurement; accuracy degrades in cloud-persistent and heterogeneous canopy regions.

### 15. Global Carbon Atlas / Global Carbon Budget
- **URL:** https://globalcarbonatlas.org/ ; https://www.globalcarbonproject.org/carbonbudget/
- **License:** CC BY 4.0 (data); attribution to Global Carbon Project required
- **Coverage:** National/global aggregate emissions and land-use flux estimates
- **Temporal resolution:** Annual, since 1959 (fossil) / 1850 (land-use)
- **Format:** CSV, xlsx downloads (not a live API)
- **Gaps:** Country/global-scale aggregates only — no sub-national or site-level resolution; land-use flux estimates carry wide uncertainty bands.

---

## Water Quality

### 16. EPA Water Quality Portal (see #2 above — cross-listed for water quality parameters specifically: nutrients, pH, dissolved oxygen, contaminants)

### 17. USGS National Water Quality Assessment (NAWQA) 🤖
- **URL:** https://www.usgs.gov/mission-areas/water-resources/science/national-water-quality-assessment-nawqa
- **License:** Public domain (US federal)
- **Coverage:** United States, targeted major river basins/aquifers
- **Temporal resolution:** Multi-decade monitoring cycles (rotating basin design) since 1991
- **Format:** Accessible via WQP/NWIS APIs; some study-specific data as downloadable CSV
- **Gaps:** Not continuous nationwide coverage — basins are sampled on rotating multi-year cycles, so any given watershed may have monitoring gaps of several years.

### 18. Copernicus Marine Service — Ocean Water Quality / Biogeochemistry 🤖
- **URL:** https://marine.copernicus.eu/
- **License:** Free and open, Copernicus license (attribution required)
- **Coverage:** Global oceans and major seas
- **Temporal resolution:** Daily to monthly reanalysis and near-real-time products, multi-decadal archives
- **Format:** NetCDF via API/WMS; requires free account registration
- **Gaps:** Marine/coastal only — no freshwater/inland coverage; large NetCDF files require subsetting tools; registration wall means not fully anonymous machine access.

---

## Summary: Machine-Readable Without Preprocessing (🤖)

USGS NWIS, EPA WQP, NOAA National Water Model, GBIF, iNaturalist Open Data, eBird API, ESA WorldCover, USDA CDL, Global Forest Watch, MODIS NPP (via AppEEARS), USGS NAWQA (via WQP/NWIS), Copernicus Marine Service.

**Not machine-readable without preprocessing:** HydroSHEDS (static GIS layers), IUCN Red List (manual request + GIS), GHSL (large raster reprojection), FLUXNET (per-site CSV needs gap-filling/merging), Global Carbon Atlas (static spreadsheet downloads).

## Cross-Cutting Gaps Observed

- **Geographic bias:** Nearly every global dataset here is denser in North America/Europe than in Africa, South Asia, and much of Latin America — a material gap for bioregional finance applications targeting the Global South.
- **Temporal mismatch:** Biodiversity and water-quality datasets skew toward point-in-time or irregular sampling, while carbon and land-use datasets increasingly offer continuous/annual coverage — makes cross-category time-alignment nontrivial.
- **License friction for commercial/agent use:** CC BY-NC terms (GBIF subset, eBird commercial use, IUCN) block straightforward use in for-profit bioregional finance products without separate licensing.
- **No single unified schema:** Every source uses its own taxonomy, coordinate reference system, and units — an AI agent pipeline needs a normalization layer regardless of which datasets are chosen.

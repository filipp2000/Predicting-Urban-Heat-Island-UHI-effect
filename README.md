# Predicting-Urban-Heat-Island-UHI-effect

This project focuses on building a machine learning model that predicts the Urban Heat Island (UHI) Index for New York City by combining **satellite remote sensing (Sentinel-2)**, **building footprints**, and **weather station** data. We engineer multi-scale spatial features (100–750 m buffers) and train a supervised model (Random Forest) to estimate the UHI Index at target locations. Developed for the 2025 EY Open Science AI &amp; Data Challenge to support sustainable urban planning.


## data
- **2025 EY Open Science AI Data Challenge Participant Guidance.pdf**  
  The official challenge document outlining objectives, rules, evaluation metrics, and submission format.
  
- **Training_data_uhi_index.csv**
  Input training file provided by the challenge. Contains UHI Index values (targets) and corresponding geographic coordinates (Longitude/Latitude). Used as the core training   base for feature engineering and model development.

- **merged_buildings_features_gdf.csv**  
  Contains the engineered building-related features (density in 100/250/400/750 m buffers, max height per buffer, Building Coverage Ratio—BCR, nearest building distance).

- **environmental_final_data.csv**  
  Contains satellite-based environmental features like NDVI, Albedo, NDBI, and weather data (e.g., temperature, humidity, wind, solar flux) for each UHI point.
  
- **Building_Footprints_20250316.csv**:
  NYC Building Footprints dataset downloaded from [NYC Open Data](https://data.cityofnewyork.us/City-Government/BUILDING/5zhs-2jue/about_data)


---

## notebooks
- **weather_data.ipynb**  
  Processes weather station data (temperature, humidity, wind speed/direction, solar flux) and merges time-/space-aligned summaries with UHI locations.
  
- **Building Footprints.ipynb**  
  Loads NYC building footprint data(`Building_Footprints_20250316.csv`) and computes spatial features (density, height, BCR, nearest distance). Computes spatial building features (density, max height, BCR, nearest building distance) using NYC building footprint data around each UHI point.

- **Sentinel2_GeoTIFF.ipynb**  
  Parses raw Sentinel-2 GeoTIFF files to extract band values for given lat/lon points and computes some indexes(e.g NDVI, NDBI). Aggregates values to UHI point buffers.

- **UHI_Model_Prediction_Pipeline.ipynb** *(Main)*  
  Merges all datasets, trains the Random Forest model, performs feature selection/importance, evaluates, and exports the final `submission.csv`.


---

## Model & Results
- **Model:** Random Forest Regressor (baseline).  
- **Engineered features:**  
  - HEIGHTROOF, GROUNDELEV
  - building_density_100m, building_density_250m
  - max_bldg_height_250m, max_bldg_height_400m, max_bldg_height_750m
  - BCR_100m, BCR_250m, BCR_400m, BCR_750m
  - Sentinel bands: B01, B06, NDVI, NDBI
  - Weather: Air Temp at Surface [degC], Heat Index (C), Solar Flux [W/m^2]  
- **notes**: Per the challenge rules, we **excluded latitude/longitude from the model training** to preserve generalizability across cities.
- **Score:** **R² = 0.9414**


3. Run notebooks in this order:
1) `weather_data.ipynb`
2) `Sentinel2_GeoTIFF.ipynb`  
3) `Building Footprints.ipynb`  
4) `UHI_Model_Prediction_Pipeline` → outputs final model

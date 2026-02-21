# 🔥 SPATIAL DISTRIBUTION – FWI

⚠️ Only WRF data is used in this visualization.

- Using `wrftoxlsx_domain.py`, domain temperature and wind speed data corresponding to wildfire ignition time steps were extracted from the `wrfout_d2` output file in Excel format (`domain_000`).

- Using NCL (`scripts/RH_NCL/rhdomain.ncl`), domain relative humidity data corresponding to wildfire ignition time steps were converted from `wrfout_d2` output into CSV format (`rh_000.csv`).

- In the `FWI/WRF_DATAMERGE/domain` directory, `domain_000` and `rh_000` datasets were merged for each station, generating `rasterfwi_istasyon.xlsx`.

- Using `fwidomain.py`, `rasterfwi_istasyon.xlsx` was converted into a raster file (`rasterfwi_istasyon.tif`).

- Using `fwiraster.R`, a new FWI raster dataset (`fwi_result_istasyon.tif`) was calculated from the station raster file (`rasterfwi_istasyon.tif`). Example `.tif` files are available in `FWI/WRF_DATAMERGE/domain/tif`.

- Using `fwi_görsel.py`, spatial distribution maps for each wildfire ignition time step were generated (`fwi_harita_istasyon.png`) from `fwi_result_istasyon.tif`.

---

# 🔥 TIME SERIES – FWI

⚠️ WRF, station, and ERA5 datasets are used in this visualization.

## 🔹 WRF Data Processing

- Relative humidity time series were converted to CSV format (`rh_timeseries_point_i_j.csv`) using NCL (`scripts/RH_NCL/rh.ncl`).
- Temperature (T) and wind speed (WS) time series were converted from NetCDF (.nc) to Excel format (`yangınij_d2.xlsx`) using `wrftoxlsx.py`.
- In the `FWI/WRF_DATAMERGE/time_series` directory, `rh_timeseries_point_i_j.csv` and `yangınij_d2.xlsx` were transferred into an Excel template (`wrf_yangın.xlsx`).

## 🔹 Station Data Processing

- Temperature, relative humidity, and wind data from the two most representative stations for each wildfire ignition point were downloaded in Excel format (`istasyon.xlsx`).
- Data source: https://www.weather.gov/wrh/hazards?obs=true&wfo=lox&basemap=OpenStreetMap&boundaries=true,false&obs_popup=true
- The content of `istasyon.xlsx` was transferred into the calculation Excel template (`yangın_istasyon.xlsx`) located in `FWI/IST_DATAMERGE/time_series`.

## 🔹 ERA5 Data Processing

- Using MATLAB (`scripts/ERA5_MATLAB/truv.m` and `scripts/ERA5_MATLAB/tp.m`), ERA5 NetCDF datasets (`truv.nc` and `tp.nc`) were converted into time series Excel format (`truv_yangın.xlsx` and `tp_yangın.xlsx`) by selecting grid points corresponding to wildfire ignition locations.
- In `FWI/ERA5_DATAMERGE/time_series`, these datasets were merged into the Excel template (`yangın_era5.xlsx`).
  - `truv`: temperature, relative humidity, u and v wind components
  - `tp`: total precipitation

## 🔹 Data Integration and FWI Calculation

- `wrf_yangın.xlsx`, `yangın_istasyon.xlsx`, and `yangın_era5.xlsx` were merged within the `FWICAL` directory.
- Using `fwi_cal.R`, FWI values were calculated for each wildfire and exported as:
  - `fwiout_yangın_istasyon/wrf/era5.xlsx`
- These outputs were then merged into a single Excel file (`fwiout_yangın.xlsx`).
- Using `fwipltsta.R`, the temporal distribution of FWI values for each wildfire was plotted (`fwiseries_yangın.png`).

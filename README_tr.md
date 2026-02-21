# FWI-Calculation
# 🔥 UZAYSAL DAĞILIM – FWI

⚠️ Bu görselde sadece WRF verisi kullanılmıştır.

- `wrftoxlsx_domain.py` ile `wrfout_d2` çıktı verisinden yangın başlangıç zaman adımlarına ait sıcaklık ve rüzgar şiddeti domain verisi Excel formatında çekilmiştir (`domain_000`).

- NCL kullanılarak (`scripts/RH_NCL/rhdomain.ncl`) `wrfout_d2` çıktı verisinden yangın başlangıç zaman adımlarına ait bağıl nem domain verisi CSV formatına çevrilmiştir (`rh_000.csv`).

- `FWI/WRF_DATAMERGE/domain` klasöründe `domain_000` ve `rh_000` verileri her istasyon için birleştirilerek `rasterfwi_istasyon.xlsx` Excel verisi oluşturulmuştur.

- `fwidomain.py` ile `rasterfwi_istasyon.xlsx` verisi `rasterfwi_istasyon.tif` dosyasına dönüştürülmüştür.

- `fwiraster.R` ile istasyonların raster verisi (`rasterfwi_istasyon.tif`) kullanılarak yeni FWI raster verisi (`fwi_result_istasyon.tif`) hesaplatılmıştır. Kullanılan `.tif` dosyalarının örneği `FWI/WRF_DATAMERGE/domain/tif` klasöründe bulunmaktadır.

- `fwi_görsel.py` ile `fwi_result_istasyon.tif` dosyaları kullanılarak her bir yangın başlangıç zaman adımı için uzaysal dağılım görseli oluşturulmuştur (`fwi_harita_istasyon.png`).

---

# 🔥 ZAMANSAL DAĞILIM – FWI

⚠️ Bu görselde WRF, istasyon ve ERA5 verisi kullanılmıştır.

## 🔹 WRF Verisinin Elde Edilmesi

- NCL ile (`scripts/RH_NCL/rh.ncl`) bağıl nem zaman serisi CSV formatına (`rh_timeseries_point_i_j.csv`) dönüştürülmüştür.
- `wrftoxlsx.py` ile sıcaklık (T) ve rüzgar şiddeti (WS) zaman serisi NetCDF (.nc) formatından Excel formatına (`yangınij_d2.xlsx`) çevrilmiştir.
- `FWI/WRF_DATAMERGE/time_series` klasöründe `rh_timeseries_point_i_j.csv` ve `yangınij_d2.xlsx` dosyaları Excel şablonuna (`wrf_yangın.xlsx`) aktarılmıştır.

## 🔹 İstasyon Verisinin Elde Edilmesi

- Yangın başlangıç noktalarını temsil eden en uygun ikişer istasyonun sıcaklık, bağıl nem ve rüzgar verisi Excel formatında indirilmiştir (`istasyon.xlsx`).
- Veri kaynağı: https://www.weather.gov/wrh/hazards?obs=true&wfo=lox&basemap=OpenStreetMap&boundaries=true,false&obs_popup=true
- `istasyon.xlsx` içeriği `FWI/IST_DATAMERGE/time_series` klasöründeki Excel şablonuna (`yangın_istasyon.xlsx`) aktarılmıştır.

## 🔹 ERA5 Verisinin Elde Edilmesi

- MATLAB kullanılarak (`scripts/ERA5_MATLAB/truv.m` ve `scripts/ERA5_MATLAB/tp.m`) ERA5 NetCDF verileri (`truv.nc` ve `tp.nc`) yangın başlangıç noktalarına karşılık gelen grid noktaları seçilerek zaman serisi olarak Excel formatına dönüştürülmüştür (`truv_yangın.xlsx` ve `tp_yangın.xlsx`).
- `FWI/ERA5_DATAMERGE/time_series` klasöründe bu dosyalar Excel şablonuna (`yangın_era5.xlsx`) aktarılmıştır.
  - `truv`: sıcaklık, bağıl nem, u ve v rüzgar bileşenleri
  - `tp`: toplam yağış

## 🔹 Veri Birleştirme ve FWI Hesabı

- `wrf_yangın.xlsx`, `yangın_istasyon.xlsx` ve `yangın_era5.xlsx` dosyaları `FWICAL` klasöründe birleştirilmiştir.
- `fwi_cal.R` kullanılarak bu dosyalardan her yangın için FWI değerleri hesaplanmış ve çıktı olarak:
  - `fwiout_yangın_istasyon/wrf/era5.xlsx` dosyaları üretilmiştir.
- Bu dosyalar daha sonra tek bir Excel dokümanında birleştirilmiştir (`fwiout_yangın.xlsx`).
- `fwipltsta.R` ile `fwiout_yangın.xlsx` kullanılarak her yangın için FWI değerlerinin zamansal dağılımı çizdirilmiştir `fwiseries_yangın.png`. 

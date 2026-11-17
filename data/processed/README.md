# Processed data

| File | Content |
|---|---|
| `weather_PI_2015_2025.csv.gz` | Daily weather for the 224 municipalities of Piauí (2015–2025), from the NASA POWER API (power.larc.nasa.gov), at each municipal seat. Variables: `T2M_MAX` (max temperature, °C), `T2M` (mean temperature, °C), `RH2M` (relative humidity, %), `PRECTOTCORR` (precipitation, mm/day), `WS2M` (wind speed, m/s). 900,032 rows. Downloaded on 2026-11-11. |
| `dataset_integrated_PI_2015_2025.csv.gz` | **Integrated dataset** built in Section 5 of the notebook: one row per municipality-day (900,032 rows), with `mun_key` (municipality name, normalized), `date`, the five weather variables above and `fire` (1 if at least one active fire was detected by INPE's reference satellite that day, 0 otherwise; 4.32% positives). No missing values. |

Note: `fire = 0` means "no detection", not certainty that no fire occurred.

Load directly in Python:

    import pandas as pd
    url = "https://raw.githubusercontent.com/MonnikLuianne/wildfire-risk-piaui/main/data/processed/dataset_integrated_PI_2015_2025.csv.gz"
    data = pd.read_csv(url, parse_dates=["date"])

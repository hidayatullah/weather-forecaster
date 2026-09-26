# Official Weather Data Sources

Only free, official sources. No commercial weather APIs. If a source isn't reachable in the current environment, say so honestly rather than substituting a non-official one.

## Contents
1. United States: NWS API
2. United States: other NOAA products
3. Climate normals and seasonal context
4. International official agencies
5. Retrieval tips
6. Checking that data is fresh

---

## 1. United States: NWS API (api.weather.gov)

Free, no key. Requires a descriptive `User-Agent` header when called from code (e.g. `User-Agent: (weather-forecaster-skill, contact@example.com)`).

| Need | Endpoint |
|---|---|
| Find the forecast office and grid for a location | `https://api.weather.gov/points/{lat},{lon}` (the response includes `forecast`, `forecastHourly`, and `forecastGridData` URLs) |
| 7-day forecast (12-hour periods) | the `forecast` URL from `/points` |
| Hourly forecast | the `forecastHourly` URL from `/points` |
| Raw gridded data (wind gusts, sky cover, etc.) | the `forecastGridData` URL from `/points` |
| Active alerts at a point | `https://api.weather.gov/alerts/active?point={lat},{lon}` |
| Active alerts for a state | `https://api.weather.gov/alerts/active?area={STATE}` |
| Nearby observation stations | `https://api.weather.gov/points/{lat},{lon}/stations` |
| Latest observation | `https://api.weather.gov/stations/{stationId}/observations/latest` |
| Area Forecast Discussion (forecaster reasoning; great for the "why") | `https://api.weather.gov/products/types/AFD/locations/{officeId}` |

Latitude and longitude should use no more than 4 decimal places. The API covers the US and its territories only.

Human-readable equivalents: `https://forecast.weather.gov/MapClick.php?lat={lat}&lon={lon}` and `https://www.weather.gov`.

## 2. United States: other NOAA products

- **Radar:** https://radar.weather.gov
- **Storm Prediction Center (SPC):** https://www.spc.noaa.gov. Convective outlooks (severe thunderstorm and tornado risk, Days 1–8), watches, and mesoscale discussions.
- **National Hurricane Center (NHC):** https://www.nhc.noaa.gov. Tropical outlooks, advisories, and the forecast cone. The Central Pacific Hurricane Center covers Hawaii.
- **Weather Prediction Center (WPC):** https://www.wpc.ncep.noaa.gov. Surface analysis maps, excessive rainfall outlooks, winter weather, and national forecast charts.
- **Climate Prediction Center (CPC):** https://www.cpc.ncep.noaa.gov. 6–10 day, 8–14 day, monthly, and seasonal outlooks, plus ENSO (El Niño/La Niña) diagnostic discussions.
- **Marine forecasts:** https://www.weather.gov/marine
- **Aviation Weather Center:** https://aviationweather.gov (METARs, TAFs, and aviation hazards).

## 3. Climate normals and seasonal context

Use these for "too far out to forecast" questions, such as what March in Austin is usually like.

- **NWS NOWData** (daily and monthly climate data and normals by forecast office): reachable from each office's "Climate" section on weather.gov.
- **NOAA NCEI U.S. Climate Normals (1991–2020):** https://www.ncei.noaa.gov/products/land-based-station/us-climate-normals
- **NCEI Climate at a Glance** (historical temperature and precipitation series): https://www.ncei.noaa.gov/access/monitoring/climate-at-a-glance/
- **ENSO status:** the CPC ENSO diagnostic discussion (updated monthly).
- **CPC week 3–4 outlook** (linked from the CPC home page, https://www.cpc.ncep.noaa.gov) covers roughly 15–28 days out, past the 8–14 day outlook. It gives odds of above- or below-normal temperature and precipitation, not daily weather.
- **CPC 6–10 and 8–14 day discussion (text, with a state-by-state table):** https://www.cpc.ncep.noaa.gov/products/predictions/610day/fxus06.html. This is the easiest way to read the outlook for a state in text.
- **Daily normals for a specific date:** NWS NOWData (from each forecast office's Climate page, or https://www.weather.gov/wrh/climate) gives the normal high and low for any day. If you can't retrieve the exact daily normal, say so rather than estimating it.

## Holiday calendars

- **US federal holidays:** US Office of Personnel Management, https://www.opm.gov/policy-data-oversight/pay-leave/federal-holidays/
- **US state holidays:** the state government's official holiday list.
- **Other countries:** the national government's official public holiday list.

Always state the normals period (e.g. "1991–2020 normals") so people know these are averages, not predictions.

## 4. International official agencies (free public forecasts)

- **WMO World Weather Information Service:** https://worldweather.wmo.int. Official forecasts from national services worldwide, and a good starting point.
- **Canada:** Environment and Climate Change Canada, https://weather.gc.ca
- **United Kingdom:** Met Office, https://www.metoffice.gov.uk
- **Pakistan:** Pakistan Meteorological Department, https://www.pmd.gov.pk
- **India:** India Meteorological Department, https://mausam.imd.gov.in
- **Australia:** Bureau of Meteorology, https://www.bom.gov.au
- **Germany:** Deutscher Wetterdienst, https://www.dwd.de
- **France:** Météo-France, https://meteofrance.com
- **Japan:** Japan Meteorological Agency, https://www.jma.go.jp
- **Europe-wide warnings:** Meteoalarm, https://www.meteoalarm.org

For other countries, search for the national meteorological service by name and confirm it is the government agency before using it. If no free official forecast exists for a place, say "I don't know" rather than using an unofficial source.

## 5. Retrieval tips

- Prefer the most specific official product: point forecast, then office forecast, then regional outlook.
- Record the issuance time of whatever you use and state it.
- For "why" questions, the NWS Area Forecast Discussion explains the forecaster's reasoning in technical language, which is ideal for enthusiasts. Paraphrase it for general audiences.
- If web fetching of API URLs is restricted in the current environment, use web search to reach the official human-readable pages instead, and still cite them.

## 6. Checking that data is fresh

When fetched through web search and page fetching, forecast.weather.gov pages are often served from a stale cache: days or even months old, with no visible warning. Check freshness before using any value:

- **Check the timestamps on every page.** Look at the page's creation date (`meta-DC.date.created`), the forecast's "Last Update" and "Forecast Valid" times, the XML `creation-date`, and the observation's "Last update" time. If the page is from an earlier day, don't use it; try another format or URL.
- **The XML formats were usually fresher than the HTML pages:**
  - `FcstType=digitalDWML` has hourly temperature, dew point, wind speed, gusts, direction in degrees, sky cover, rain chance, and weather type. It feeds the hourly widget.
  - `FcstType=dwml` has 12-hour periods, the latest observation, and the list of active hazards with links to their text.
- **The tabular page (`FcstType=digital`) links to the `digitalDWML` URL,** so fetch it even when its own table is stale, then fetch the XML link it contains.
- **A point page reached through a different URL** (a different lat/lon precision or extra query parameters) may be fresh when the usual URL is cached.
- **Observation history pages** (`obhistory`, the WRH time series) were stale or rendered by JavaScript and unusable. If the hour-before observation can't be found, mark it "No data" rather than guessing.
- **Units:** DWML observation wind speeds are in knots. Convert to mph (knots × 1.15, rounded). The digital forecast's wind speeds are already in mph.
- **Temperatures can be missing:** some station observations report no air temperature, and some DWML observations include only apparent ("feels like") temperature. Show exactly what was reported.

# Condition Icons and Colors

All widgets use this table. Every icon is a Tabler outline icon (`ti ti-NAME`), checked against Tabler Icons 3.x. Never use `-filled` icons or icons that aren't in this table.

## Colors

Colors group conditions by what they're made of or what they do. They are mid-tone shades that stay readable in both light and dark mode. Apply the color as an inline style on the icon: `style="color:#EF9F27"`.

| Family | Color | Used for |
|---|---|---|
| Sun and light | `#EF9F27` | Sunny, clear by day, sunrise and sunset, and optical effects like rainbows and halos (explanations only) |
| Night | `#7F77DD` | Clear or mostly clear at night |
| Clouds, fog, and wind | `#888780` | Clouds, mist, fog, haze, calm, breezy, windy, gusty |
| Rain and water | `#378ADD` | Drizzle, rain, showers, humid, dew, flooding |
| Snow, ice, and cold | `#1D9E75` | Snow, sleet, hail, freezing rain or fog, frost, ice, cold |
| Possible thunderstorms | `#D4537E` | Any *chance* or *possibility* of thunderstorms ("t-storm possible", "slight chance", "chance") |
| Storms | `#E24B4A` | Thunderstorms likely or occurring, severe storms, tornadoes, gales, tropical storms, hurricanes |
| Heat and dryness | `#D85A30` | Hot, heat wave, dry |
| Smoke, dust, and ash | `#BA7517` | Smoke, dust, blowing dust, sandstorms, volcanic ash |

Storm red matches the warning banner, so it's saved for thunderstorms that are likely or occurring and for dangerous weather. A mere chance of a thunderstorm uses pink, so it can't be confused with a warning or with night (purple).

Colors never carry meaning on their own. Every icon keeps its text label.

## Condition table

When a condition fits several rows, use the most specific or most severe one. "Close match" means no exact icon exists; the label under the icon carries the exact condition.

| Condition | Icon | Color family | Note |
|---|---|---|---|
| Sunny, mostly sunny, fair or clear (day), sky cover 0–30% | `ti-sun` | Sun | |
| Clear or mostly clear (night) | `ti-moon` | Night | |
| Partly sunny or partly cloudy, sky cover 31–60% | `ti-cloud` | Clouds | Close match |
| Mostly cloudy or overcast, sky cover 61–100% | `ti-cloud` | Clouds | |
| Drizzle | `ti-droplet` | Rain | |
| Rain, light rain, showers, rain likely | `ti-cloud-rain` | Rain | |
| Heavy rain or downpour | `ti-droplets` | Rain | |
| Thunderstorm possible (slight chance or chance) | `ti-cloud-storm` | Possible thunderstorms | |
| Thunderstorms likely or occurring | `ti-cloud-storm` | Storms | |
| Severe thunderstorm | `ti-cloud-bolt` | Storms | |
| Lightning or thunder | `ti-bolt` | Storms | |
| Tornado, funnel cloud, or waterspout | `ti-tornado` | Storms | |
| Tropical storm or hurricane conditions | `ti-storm` | Storms | |
| Freezing drizzle | `ti-droplet-half` | Snow and ice | Close match |
| Freezing rain | `ti-temperature-snow` | Snow and ice | Close match |
| Flurries | `ti-snowflake` | Snow and ice | |
| Snow or snow showers | `ti-cloud-snow` | Snow and ice | |
| Heavy snow | `ti-snowman` | Snow and ice | Close match |
| Wintry mix or snow squall | `ti-cloud-snow` | Snow and ice | Close match |
| Sleet or snow grains | `ti-grain` | Snow and ice | |
| Hail or graupel | `ti-circles` | Snow and ice | Close match |
| Blizzard | `ti-snowflake` | Snow and ice | Close match |
| Blowing snow | `ti-wind` | Snow and ice | Close match |
| Frost | `ti-snowflake` | Snow and ice | Close match |
| Ice or black ice | `ti-ice-skating` | Snow and ice | Close match |
| Mist | `ti-mist` | Clouds | |
| Fog | `ti-cloud-fog` | Clouds | |
| Freezing fog | `ti-cloud-fog` | Snow and ice | Close match |
| Haze | `ti-haze` | Clouds | |
| Smoke | `ti-flame` | Smoke and dust | Close match |
| Dust | `ti-dots` | Smoke and dust | Close match |
| Blowing dust, sandstorm, or dust storm | `ti-sun-wind` | Smoke and dust | Close match |
| Volcanic ash | `ti-volcano` | Smoke and dust | |
| Calm (as a condition) | `ti-wind-off` | Clouds | |
| Breezy or windy | `ti-wind` | Clouds | |
| Gusty | `ti-windsock` | Clouds | |
| Gale or damaging winds | `ti-windsock` | Storms | |
| Hot | `ti-temperature-sun` | Heat | |
| Heat wave or extreme heat | `ti-temperature-plus` | Heat | |
| Cold | `ti-temperature-snow` | Snow and ice | |
| Bitter cold or dangerous wind chill | `ti-temperature-minus` | Snow and ice | |
| Flooding | `ti-flood` | Rain | |

When a forecast combines sky and weather ("Mostly sunny, late showers"), use the icon for the most significant weather in the period (here, showers), unless it's only a slight chance, in which case use the sky icon.

## Data-state icons (not weather)

| State | Icon | Color |
|---|---|---|
| No data | `ti-help-circle` | `var(--text-muted)` |
| Passed period or day | `ti-clock` | `var(--text-muted)` |
| Wind direction arrow | `ti-arrow-up` (rotated) | inherits the text color |

## Alert banner icons

| Alert | Icon | Colors |
|---|---|---|
| Warning | `ti-alert-octagon` | danger colors (red) |
| Watch | `ti-alert-triangle` | warning colors (amber) |
| Advisory | `ti-info-circle` | silver (neutral surface, strong border, secondary text) |

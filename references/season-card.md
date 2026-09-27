# Season Card Specification

When a date is beyond the forecast range, there is no forecast to show, and the skill never makes one up. Instead it shows the **season**: what the weather is typically like on those dates, based on the **30-year climate normals (1991–2020)**, plus records and typical conditions from official sources. In this skill, "season" always means the 30-year average for those specific dates at that place.

The season card is a single card (like the day card) that summarizes a day, weekend, long weekend, or week. It must look the same every time. Build it only from `assets/season-card-template.html` and the snippets below. Every number must come from an official source retrieved in the current conversation. Never estimate a normal you couldn't retrieve.

The design was approved from live examples (a holiday long weekend and a regular weekend, both beyond the forecast range).

## When to use it

- Any requested date or date range beyond the NWS forecast range (roughly 7 days).
- Instead of a widget full of "No data" columns. If only the last one or two periods are beyond range, use the regular widget with "No data" columns instead.

## Data sources (in order of preference)

1. **The local NWS office's monthly climate table** for its official station, which gives normal high, low, and precipitation for each day, plus records and years. Some NWS offices publish these as monthly PDF tables on their climate pages. Find one by searching for the office and station name with the month (e.g. "NWS [office] [station] October normals"), and check that the station is the right one. Not every office publishes these tables.
2. **NWS NOWData** (the Climate section of each office's page) or **NCEI U.S. Climate Normals** (1991–2020) for daily normals.
3. **The NWS office's climate summary** (e.g. "[City] Climate Summary") for typical conditions: prevailing winds, wet and dry months, fog and low clouds, severe weather season, flooding, tropical, snow, and ice risks.
4. **The CPC 8–14 day and week 3–4 outlooks** for any part of the range they cover. Mention these in the reply text, not on the card.

Always name the station and roughly how far it is from the place when they differ (for example, "the city's main airport station, about 10 miles away").

## Placeholders

| Placeholder | Fill with |
|---|---|
| `{{SR_SUMMARY}}` | "Typical weather, not a forecast: 1991 to 2020 averages and records for [station], [City], [ST], for the weekend of October 16 to 18, 2026, with typical conditions for mid-October." |
| `{{LOCATION}}` | "City, ST" for US places. |
| `{{DATE_LABEL}}` | Regular dates: `<span class="sub">Weekend · Oct 16–18</span>` (or "Oct 16", "Week · Oct 12–18"). Long weekend: `<span class="badge"><i class="ti ti-calendar-star" style="font-size:15px" aria-hidden="true"></i>Long weekend · Oct 9–12</span>`. |
| `{{SEASON_LABEL}}` | Early, mid-, or late plus the month: "Mid-October season" (days 1–10 early, 11–20 mid, 21–end late; for ranges, use the middle date). |
| `{{TYPICAL_HIGH}}`, `{{TYPICAL_LOW}}` | The range of the daily normal highs (and lows) across the dates, e.g. "82–83°". A single value if they're all the same: "84°". |
| `{{METRIC_COUNT}}` | The number of metric boxes (2–4). |
| `{{METRICS}}` | Metric box snippets (below), only for data the source provides and that matters for the season. |
| `{{TYPICAL_CONDITIONS}}` | The typical conditions section (below), or empty if the sources don't describe any. |
| `{{RECORD_HIGH}}`, `{{RECORD_HIGH_DATES}}` | The highest record high across the dates, e.g. "95°", and the date(s) and year(s): "Oct 17, 1993". Separate several with " · ". |
| `{{RECORD_LOW}}`, `{{RECORD_LOW_DATES}}` | The same for the lowest record low. |
| `{{EXTREME_LINE}}` | `<div class="sub" style="margin-top:14px">Wettest day on record for these dates: 6.24 in on Oct 17, 1998</div>`, or the snowiest day in snow season. Empty if not available. |
| `{{CAPTION}}` | The sources, e.g. "NWS [office]: [station] October climate table (1991–2020 normals and records) and [City] Climate Summary." |
| `{{SOURCE_URL}}`, `{{SOURCE_LINK_TEXT}}` | The main source's URL, with link text such as "View the climate table". |

## Metric boxes

```html
<div class="met"><div class="ml"><i class="ti {{ICON}}" style="font-size:15px;color:{{ICON_COLOR}}" aria-hidden="true"></i>{{LABEL}}</div><div class="mv">{{VALUE}}</div></div>
```

Include a box only when the source has the number and it matters for that place and season. Never show a box with a guessed value.

| Box | When | Icon and color | Value |
|---|---|---|---|
| Avg rain per day | Daily precipitation normals are available | `ti-droplet`, `#378ADD` | Range, e.g. "0.13–0.15 in" |
| Avg rain, whole trip | Same | `ti-droplets`, `#378ADD` | The sum of the daily normals, rounded to the nearest tenth: "About 0.4 in" (label "whole weekend", "whole week", or "whole trip") |
| Avg snow per day / whole trip | The daily snowfall normal is above zero for any of the dates | `ti-snowflake`, `#1D9E75` | Same style as rain |
| Other averages (wind, humidity, days with rain) | Only when an official source gives them for these dates | from `icons.md` | As given |

## Typical conditions

Add a section for conditions the official climate sources describe as typical for that place and time of year:

```html
<div class="sub" style="margin:0 0 8px">Typical conditions this time of year</div>
<div class="tc">
{{CONDITION_ROWS}}
</div>
```

Each row uses one of three levels. **Only watch-level and warning-level hazards get color.** Everything else, including advisory-level conditions and general weather, is silver.

- **Warning level (red):** dangerous conditions that are typical for the season, such as flash flooding in a flood-prone region during its wet season, snowstorms or blizzards in a snowy winter, hurricane season on the coast, or extreme heat.
  ```html
  <div class="ti-row warn"><i class="ti ti-alert-octagon" style="font-size:18px;margin-top:1px" aria-hidden="true"></i><div><div style="font-size:14px;font-weight:500">{{TITLE}}</div><div>{{TEXT}}</div></div></div>
  ```
- **Watch level (amber):** hazards that are possible but less common this time of year, such as occasional severe storms or an early freeze.
  ```html
  <div class="ti-row watch"><i class="ti ti-alert-triangle" style="font-size:18px;margin-top:1px" aria-hidden="true"></i><div><div style="font-size:14px;font-weight:500">{{TITLE}}</div><div>{{TEXT}}</div></div></div>
  ```
- **General weather and advisory level (silver):** notable but not dangerous, such as windy afternoons, morning fog or low clouds, humidity, or prevailing winds.
  ```html
  <div class="ti-row silver"><i class="ti {{ICON}}" style="font-size:18px;margin-top:1px;color:#888780" aria-hidden="true"></i><div><div class="h" style="font-size:14px;font-weight:500">{{TITLE}}</div><div>{{TEXT}}</div></div></div>
  ```

Order the rows red first, then amber, then silver, with at most four rows. Keep each text to one or two sentences, and don't claim more than the source says (for example, if the source only describes winter wind shifts, don't apply them to October).

## Reply text around the card

Start with the holiday line if it's a long weekend. Say plainly that it's too far out to forecast and that the card shows the season (30-year averages and records). Add a few bullets: what the typical high and low mean for their plans, the rain or snow picture, the widest swings on record, and the CPC outlook for any part of the range it covers. Close with an offer to check the real forecast once it's within about 5–7 days, with the date that will happen.

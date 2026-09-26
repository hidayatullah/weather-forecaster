# Day Card Widget Specification

The day card gives the overall picture of the day in one card: current conditions, humidity, dew point, and wind, then the forecast for the next two periods (such as "This afternoon" and "Tonight"). It must look the same every time. Build it only from `assets/day-card-template.html` and the exact snippets below. Fill in the data; don't change the layout, styling, class names, colors, icons, or wording patterns, and don't add or remove elements beyond what these rules allow. Every number must come from data retrieved in the current conversation.

The design was approved from a live Round Rock, TX example.

## How to build it

1. Read `assets/day-card-template.html`.
2. Replace every `{{PLACEHOLDER}}` using the rules below.
3. Render the result with the inline visual tool (in claude.ai, the Visualizer's `show_widget`, with a title such as `round_rock_day_card`), passing the filled-in template exactly as it is.

## Data sources

- **Current conditions:** the latest observation on the NWS point forecast page ("Current conditions at ..."), or the observation section of `FcstType=dwml`. Check that its "Last update" time is from the last hour or so (see `data-sources.md`, section 6).
- **Periods:** the first two periods of the NWS 7-day forecast on the same page (e.g. "This Afternoon" and "Tonight"), including their high or low, short forecast, and wind wording.

## Placeholders

| Placeholder | Fill with |
|---|---|
| `{{SR_SUMMARY}}` | "Current weather and today's forecast for Round Rock, TX, on Saturday, September 26, 2026, from National Weather Service data." Add ", with an active hurricane watch" (or similar) when a banner is shown. |
| `{{BANNER}}` | The same banner snippets as the hourly widget (see `hourly-widget.md`, "Banner snippets"): red for warnings, amber for watches, silver for advisories. Empty string if there are no alerts. The banner sits above the card. |
| `{{LOCATION}}` | "City, ST" for US places. Never the full state name. |
| `{{CORNER}}` | Short weekday and observation time with time zone, e.g. "Sat, 2:35 PM CDT". If there's no current reading: short weekday and "forecast only", e.g. "Sat, forecast only". |
| `{{CURRENT}}` | One of the current-conditions snippets below. |
| `{{HUMIDITY}}` | e.g. "48%", or "--" if not reported. |
| `{{DEWPOINT}}` | e.g. "66°", or "--" if not reported. |
| `{{WIND}}` | One of the wind snippets below, for the observed wind. |
| `{{PERIOD_1}}`, `{{PERIOD_2}}` | Period snippets for the next two NWS forecast periods. |
| `{{CAPTION}}` | e.g. "Now: KEDC observation. Forecast: NWS Austin/San Antonio, issued 1:24 PM CDT." If there's no current reading: "Forecast: NWS Austin/San Antonio, issued 1:24 PM CDT." |
| `{{SOURCE_URL}}` | The weather.gov point forecast page for the location. |

## Current-conditions snippets

**With a current reading:**
```html
<div style="display:flex;align-items:center;gap:16px;margin-top:12px">
<i class="ti {{ICON}}" style="font-size:36px;color:{{ICON_COLOR}}" aria-hidden="true"></i>
<span class="big">{{TEMP}}°</span>
<div><div style="font-size:17px">{{CONDITION}}</div><div class="sub" style="font-size:15px">Feels like {{FEELS}}°</div></div>
</div>
```
- `{{CONDITION}}` is the observation's reported weather in sentence case ("Fair", "Partly cloudy", "Light rain"). `{{ICON}}` and `{{ICON_COLOR}}` come from the condition table in `icons.md` ("Fair" or "Clear" is `ti-sun` by day and `ti-moon` at night).
- `{{FEELS}}` is the heat index or wind chill if reported. If neither is reported, remove the whole "Feels like" line.

**No current reading, or the reading is stale:**
```html
<div style="display:flex;align-items:center;gap:16px;margin-top:12px">
<i class="ti ti-help-circle" style="font-size:36px;color:var(--text-muted)" aria-hidden="true"></i>
<span class="big" style="color:var(--text-muted)">--</span>
<div><div style="font-size:17px">No current reading</div><div class="sub" style="font-size:15px">Nearest station hasn't reported</div></div>
</div>
```
In this case, set humidity, dew point, and wind to "--".

## Wind snippets (for the metric card)

- **Calm:** `Calm`
- **Wind from one direction:** `<i class="ti ti-arrow-up" style="transform:rotate({{DEG}}deg);font-size:16px" aria-label="wind from {{DIR_WORDS}}"></i>{{DIR}} {{SPEED}} mph`
- **With gusts:** add `, gusts {{GUST}}` after the speed.
- **Not reported:** `--`

The arrow follows the same convention as the hourly widget: it points toward where the wind comes from (N up, E right, S down, W left), and the compass label always sits beside it.

## Period snippets

```html
<div><div class="pl">{{PERIOD_NAME}}</div><div class="pv">{{HIGH_OR_LOW}}</div><div class="pd">{{SHORT_FORECAST}} ·{{PERIOD_WIND}}</div></div>
```

- `{{PERIOD_NAME}}` is the NWS period name in sentence case: "This afternoon", "Today", "Tonight", "Sunday", "Sunday night".
- `{{HIGH_OR_LOW}}` is "High 95°" for daytime periods and "Low 73°" for night periods.
- `{{SHORT_FORECAST}}` is the NWS short forecast in sentence case ("Mostly cloudy", "Partly cloudy", "Chance showers"). If there's a rain chance of 20% or more, add it after a comma, e.g. "Chance showers, 40%".
- `{{PERIOD_WIND}}` uses the arrow class `ar`:
  - **One direction:** `<i class="ti ti-arrow-up ar" style="transform:rotate({{DEG}}deg)" aria-label="wind from {{DIR_WORDS}}"></i>{{DIR}} {{SPEED}} mph`
  - **Wind that shifts** (NWS says "becoming"): `<i class="ti ti-arrow-up ar" style="transform:rotate({{DEG1}}deg)" aria-label="wind from {{DIR1_WORDS}}"></i>{{DIR1}}, then<i class="ti ti-arrow-up ar" style="transform:rotate({{DEG2}}deg)" aria-label="wind from {{DIR2_WORDS}}"></i>{{DIR2}} {{SPEED}} mph`
  - **Calm becoming a direction:** `Calm, then<i ...></i>{{DIR}} {{SPEED}} mph`
  - **Speed ranges** keep the NWS wording: "5–10 mph". Add ", gusts {{GUST}}" when the NWS mentions gusts.

For compass points, use the degrees table in `hourly-widget.md`: N=0, NNE=22.5, NE=45, ENE=67.5, E=90, ESE=112.5, SE=135, SSE=157.5, S=180, SSW=202.5, SW=225, WSW=247.5, W=270, WNW=292.5, NW=315, NNW=337.5.

## Reply text around the card

The reply follows the brief update format in SKILL.md: a few bullets for what the card doesn't show (for example, when rain is most likely, or the peak feels-like temperature), then a short paragraph and the follow-up question. If the observation station is some distance from the place, say so in a sentence.

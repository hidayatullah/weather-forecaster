# Weekend Widget Specification

The weekend widget shows the weekend one NWS forecast period at a time, in the same column style as the hourly widget. It always has exactly five columns: Friday night, Saturday, Saturday night, Sunday, and Sunday night. Friday night is always included because many weekend trips start Friday evening.

It must look the same every time. Build it only from `assets/weekend-widget-template.html` and the exact snippets below. Fill in the data; don't change the layout, styling, class names, colors, icons, or wording patterns, and don't add or remove elements beyond what these rules allow. Every number must come from data retrieved in the current conversation.

The design was approved from live examples: Round Rock, TX (no alerts), Kahului, HI (watches), Point Pleasant Beach, NJ (warnings), and Honolulu, HI (advisory).

## How to build it

1. Read `assets/weekend-widget-template.html`.
2. Replace every `{{PLACEHOLDER}}` using the rules below.
3. Render it with the inline visual tool (in claude.ai, the Visualizer's `show_widget`, with a title such as `round_rock_weekend_widget`), passing the filled-in template exactly as it is.

## Which weekend

- **Monday through Friday before 6 PM:** the coming weekend (Friday night through Sunday night).
- **Friday evening through Sunday night:** the current weekend. Periods that have already passed stay in place as muted "Passed" columns, so the widget always has five columns.
- **If any period is beyond the forecast range,** show it with the "No data" column snippet from `hourly-widget.md` (tag "No data").

## Data source

The NWS 7-day forecast periods on the point forecast page (or the 12-hour periods in `FcstType=dwml`): each period's name, high or low, short forecast, detailed wind wording, and chance of precipitation.

## Placeholders

| Placeholder | Fill with |
|---|---|
| `{{SR_SUMMARY}}` | "Weekend forecast for Kahului, HI, from Friday night through Sunday night, September 25 to 27, 2026, with an active tropical storm watch and flood watch, from National Weather Service data." Leave out the alert clause if there are no alerts. |
| `{{LOCATION}}` | "City, ST" for US places. |
| `{{DATES}}` | Friday's date through Sunday's date: "Sep 25–27", or "Oct 30–Nov 1" across months. |
| `{{BANNER}}` | A banner snippet (see "Banners" below), or an empty string if there are no alerts. |
| `{{FRI_NIGHT}}` ... `{{SUN_NIGHT}}` | Period column snippets below. |
| `{{CAPTION}}` | "NWS Honolulu forecast, issued 6:02 AM HST." If no issue time is available, use "as of" plus the time the page was retrieved. |
| `{{SOURCE_URL}}` | The weather.gov point forecast page. |

## Period column snippets

**Forecast period:**
```html
<div class="col"><span class="t">{{LABEL}}</span><span class="tag">{{CONDITION}}</span><i class="ti {{ICON}} ic" style="color:{{ICON_COLOR}}" aria-hidden="true"></i><span class="temp">{{TEMP}}°F</span><span class="gust">{{HIGH_OR_LOW}}[ · {{POP}}% rain]</span><span class="wind">{{WIND}}</span></div>
```
- `{{LABEL}}`: "Fri night", "Sat", "Sat night", "Sun", "Sun night".
- The period happening now uses `class="col now"`, but keeps its day label (e.g. "Sat") rather than "Now".
- `{{HIGH_OR_LOW}}`: "High" for daytime periods, "Low" for night periods. Add " · 40% rain" only when the chance of precipitation is 20% or more.
- `{{CONDITION}}`: a short version of the NWS short forecast, in sentence case, 2–5 words: "Mostly cloudy", "Showers, t-storm possible", "Mostly sunny, late showers", "Morning showers, then partly sunny". When NWS says "tropical storm conditions possible", add ", tropical storm possible" (e.g. "T-storms, tropical storm possible").
- `{{ICON}}` and `{{ICON_COLOR}}`: from the condition table in `icons.md`. For night periods that are clear or mostly clear, use `ti-moon` in the night color.

**Passed period:**
```html
<div class="col"><span class="t">{{LABEL}}</span><span class="tag">Passed</span><i class="ti ti-clock ic" style="color:var(--text-muted)" aria-hidden="true"></i><span class="temp" style="color:var(--text-muted)">--</span><span class="wind">--</span></div>
```

## Wind

Use the NWS wording for the period, with the same arrow convention as the other widgets (the arrow points toward where the wind comes from, and the compass label sits beside it; see the degrees table in `hourly-widget.md` or `day-card.md`):

- **One direction:** `<i class="ti ti-arrow-up" style="transform:rotate({{DEG}}deg);font-size:16px" aria-label="wind from {{DIR_WORDS}}"></i>{{DIR}} {{SPEED}} mph`
- **With gusts:** add `, gusts {{GUST}}`. In the weekend widget, show gusts whenever the NWS mentions them, with or without alerts, because each period covers half a day.
- **Wind that shifts ("becoming"):** `<i ... DEG1 ...></i>{{DIR1}}, then<i ... DEG2 ...></i>{{DIR2}} {{SPEED}} mph`
- **Speed ranges:** keep the NWS range, e.g. "25–30 mph".
- **Calm:** `Calm`
- **No wind in the forecast** (for example, when NWS only says "tropical storm conditions possible"): `--`. Never guess.

## Banners

Same banners as the other widgets (see `hourly-widget.md`, "Banner snippets"), styled by the most severe active alert: red for warnings, amber for watches, and silver for advisories.

## Reply text around the widget

Follow the brief update format in SKILL.md: a few bullets for what the widget doesn't show (for example, the timing of rain within a period, heat index peaks, or which days an alert covers), a short paragraph, and a follow-up question.

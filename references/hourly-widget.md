# Hourly Weather Widget Specification

The hourly widget must look the same every time. Build it only from the fixed template in `assets/hourly-widget-template.html` and the exact snippets below. Fill in the data; do not change the layout, styling, class names, colors, icon set, or wording patterns, and do not add or remove elements beyond what these rules allow. Every number must come from data retrieved in the current conversation.

The template was approved from three real examples: a quiet day, a hurricane watch, and coastal flood and high wind warnings.

## How to build it

1. Read `assets/hourly-widget-template.html`.
2. Replace every `{{PLACEHOLDER}}` using the rules below.
3. Render the result with the inline visual tool (in claude.ai, the Visualizer's `show_widget`, with a title such as `city_hourly_weather`). Pass the filled-in template exactly as it is.

The widget is only for one place and one day. The weekend uses the weekend widget (see `weekend-widget.md`), the work week and next 7 days use the week widgets (see `week-widget.md`), long weekends use the long-weekend widget (see `long-weekend-widget.md`), and any date beyond the forecast range uses the season card (see `season-card.md`). For the overall picture of a day, use the day card instead (see `day-card.md`); SKILL.md explains how to choose.

## Placeholders

| Placeholder | Fill with |
|---|---|
| `{{SR_SUMMARY}}` | One sentence for screen readers: "Hourly weather for [City], [ST], from 7 AM to noon HST on Saturday, September 26, 2026, with an active hurricane watch, from National Weather Service data." Leave out the alert clause if there are no alerts. |
| `{{LOCATION}}` | "City, ST" for US places (e.g. "[City], [ST]"). Never the full state name. |
| `{{DATE}}` | Short weekday and date: "Sat, Sep 26". |
| `{{HIGH}}` | Today's forecast high, number only. |
| `{{BANNER}}` | One of the banner snippets below, or an empty string if there are no watches or warnings. |
| `{{COLUMN_1}}` to `{{COLUMN_6}}` | The hour before, now, then the next four hours, each built from a column snippet below. |
| `{{CAPTION}}` | Sources and issue time, e.g. "1 PM: [station] observation. 2–6 PM: NWS [office] forecast, issued 4:12 AM EDT." Nothing else. There is no arrow legend. |
| `{{SOURCE_URL}}` | The weather.gov point forecast page for the location. |

## Column snippets

Every column uses one of these shapes. The rain and gust lines (marked "alert lines" below) follow these rules: add the rain line when any column's chance of rain is 20% or more, and add the gust line when any column reports gusts or a watch or warning banner is shown. When one column has a line, every column that has the data has it too.

**Forecast hour:**
```html
<div class="col"><span class="t">{{HOUR}}</span><span class="tag">{{CONDITION}}</span><i class="ti {{ICON}} ic" style="color:{{ICON_COLOR}}" aria-hidden="true"></i><span class="temp">{{TEMP}}°F</span>[alert line: <span class="gust">{{POP}}% rain</span>]<span class="wind"><i class="ti ti-arrow-up" style="transform:rotate({{DEG}}deg);font-size:16px" aria-label="wind from {{DIR_WORDS}}"></i>{{DIR}} {{SPEED}} mph</span>[alert line: <span class="gust">Gusts {{GUST}}</span>]</div>
```

**Now column:** same as the forecast hour or observed hour, but with `class="col now"` and `{{HOUR}}` set to `Now`.

**Observed hour:**
```html
<div class="col"><span class="t">{{HOUR}}</span><span class="tag">Observed {{OBS_TIME}}</span><i class="ti {{ICON}} ic" style="color:{{ICON_COLOR}}" aria-hidden="true"></i><span class="temp">{{TEMP}}°F</span><span class="wind"><i class="ti ti-arrow-up" style="transform:rotate({{DEG}}deg);font-size:16px" aria-label="wind from {{DIR_WORDS}}"></i>{{DIR}} {{SPEED}} mph</span>[alert line: <span class="gust">Gusts {{GUST}}</span>]</div>
```
`{{OBS_TIME}}` is the observation time without AM/PM, e.g. "11:35".

**Observed, no temperature reported:** replace the temperature span with:
```html
<span class="temp" style="color:var(--text-muted)">--</span><span class="gust">Temp not reported</span>
```

**Observed, only "feels like" reported:** keep the temperature span with the feels-like value and add right after it:
```html
<span class="gust">Feels like</span>
```

**No data:**
```html
<div class="col"><span class="t">{{HOUR}}</span><span class="tag">No data</span><i class="ti ti-help-circle ic" style="color:var(--text-muted)" aria-hidden="true"></i><span class="temp" style="color:var(--text-muted)">--</span><span class="wind">--</span></div>
```

**Calm wind** (no direction): replace the whole wind span with:
```html
<span class="wind">Calm</span>
```

### Which column goes where

- **Column 1 (hour before):** an observation from that hour if one was retrieved, otherwise "No data".
- **Column 2 (Now):** the latest observation if it's from the current hour. If the latest observation is from the previous hour, put it in column 1 and use the current hour's forecast here.
- **Columns 3–6:** the next four forecast hours.
- **For a specific future day** ("will it rain Saturday?"): six consecutive forecast hours centered on the time that matters (the afternoon by default), with no Now column (all use plain `class="col"`).

### Condition tags and icons

`{{CONDITION}}` is a short condition in sentence case ("Sunny", "Partly sunny", "Rain likely", "Rain, t-storm possible", "Snow", "Fog"). `{{ICON}}` and `{{ICON_COLOR}}` come from the condition table in `icons.md`. For observed hours the tag is "Observed {{OBS_TIME}}", and the icon and color come from the observation's reported weather.

## Wind arrow convention

The arrow points toward the compass direction the wind is coming from, matching the label beside it: N points up, E right, S down, W left. `{{DEG}}` is the wind direction in degrees from the data (e.g. 310), used as-is. If only a compass point is available, use N=0, NE=45, E=90, SE=135, S=180, SW=225, W=270, NW=315, and the in-between points (NNE=22.5, ENE=67.5, and so on). `{{DIR}}` is the compass abbreviation, and `{{DIR_WORDS}}` spells it out for screen readers ("northwest", "east-northeast").

Never add text explaining the arrows.

## Banner snippets

Use the most severe active alert to choose the banner: warning (red), then watch (amber), then advisory (silver). Lower-level alerts are listed on the "Also active" line.

**Warning (red), when any warning is active.** Give one headline line per warning, then list the other alerts:
```html
<div style="background:var(--bg-danger);border:0.5px solid var(--border-danger);border-radius:var(--radius);padding:10px 12px;margin:0 0 12px;display:flex;gap:10px;align-items:flex-start">
<i class="ti ti-alert-octagon al" style="font-size:20px;margin-top:1px" aria-hidden="true"></i>
<div class="al" style="font-size:13px;line-height:1.5">
<div style="font-size:14px;font-weight:500">{{WARNING_NAME}} until {{END}}</div>
<div>Also active: {{OTHER_ALERTS}}. Issued by NWS {{OFFICE}}. <a class="al" href="{{ALERT_URL}}">Read the {{SHORT_NAME}} warning</a></div>
</div>
</div>
```
Repeat the headline `<div>` for each warning, and add a link per warning separated by " · ". Leave out the "Also active" text if there's nothing else.

**Watch (amber), when watches are active but no warnings.**
```html
<div style="background:var(--bg-warning);border:0.5px solid var(--border-warning);border-radius:var(--radius);padding:10px 12px;margin:0 0 12px;display:flex;gap:10px;align-items:flex-start">
<i class="ti ti-alert-triangle" style="font-size:20px;color:var(--text-warning);margin-top:1px" aria-hidden="true"></i>
<div style="font-size:13px;color:var(--text-warning);line-height:1.5">
<div style="font-size:14px;font-weight:500">{{WATCH_NAME}} in effect</div>
<div>Also active: {{OTHER_ALERTS}}. Issued by NWS {{OFFICE}}. <a href="{{ALERT_URL}}" style="color:var(--text-warning)">Read the watch</a></div>
</div>
</div>
```

**Advisory (silver), when advisories or statements are active but no watches or warnings.** Silver means "worth knowing", not a hazard; only watches and warnings get color. Give one headline line per advisory:
```html
<div style="background:var(--surface-1);border:0.5px solid var(--border-strong);border-radius:var(--radius);padding:10px 12px;margin:0 0 12px;display:flex;gap:10px;align-items:flex-start">
<i class="ti ti-info-circle" style="font-size:20px;color:var(--text-secondary);margin-top:1px" aria-hidden="true"></i>
<div style="font-size:13px;color:var(--text-secondary);line-height:1.5">
<div style="font-size:14px;font-weight:500;color:var(--text-primary)">{{ADVISORY_NAME}} until {{END}}</div>
<div>Also active: {{OTHER_ALERTS}}. Issued by NWS {{OFFICE}}. <a href="{{ALERT_URL}}" style="color:var(--text-secondary)">Read the advisory</a></div>
</div>
</div>
```
Leave out the "Also active" text if there's nothing else. Statements (such as a rip current statement) go in "Also active" and never get their own headline.

Alert names are written in sentence case ("Coastal flood warning"), and end times as "Sun 2 PM". Never hide an alert that is more serious than the headline.

## Reply text around the widget

The reply around the widget follows the brief update format in SKILL.md: a few bullets for what the widget doesn't show at a glance, a short paragraph, and the follow-up question. When a watch or warning is active, say plainly what it means (a warning means the hazard is expected, a watch means it's possible) and what it's for. Mention data gaps or stale data in a sentence, and don't repeat the numbers the widget already shows.

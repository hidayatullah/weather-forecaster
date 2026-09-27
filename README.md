# Weather Forecaster

An agent skill that turns an AI assistant into a truthful, curious weather forecaster. It presents forecasts with consistent visual widgets, communicates watches and warnings, and discusses weather science, climate patterns, history, and folklore.

The skill is triggered whenever weather comes up: direct questions ("will it rain Saturday?"), passing mentions ("it's been so hot lately"), and indirect hints where weather matters (travel, fishing, hiking, driving, outdoor events).

## Core principle: truthfulness

- Every number, alert, and forecast must come from a source retrieved in the current conversation.
- Sources and issue times are always named.
- "I don't know" is an acceptable answer when data isn't available.
- Personal interpretation is clearly labeled as such.
- Made-up data is allowed only in explicitly requested demos, and must carry the sample-data markers described in [`references/sample-data.md`](references/sample-data.md).

## Repository layout

```
SKILL.md                 Entry point: persona, behavior rules, widget selection
assets/                  Fixed HTML templates for each widget
references/              Specifications, data sources, and icon rules
```

### Widgets

Each widget is built only from its template, filled in according to its spec. Templates must not be restyled, merged, or extended.

| Widget | Used for | Template | Spec |
|---|---|---|---|
| Day card | The overall picture: current conditions plus the next two periods | [`assets/day-card-template.html`](assets/day-card-template.html) | [`references/day-card.md`](references/day-card.md) |
| Hourly widget | Hour-by-hour breakdown (six columns) | [`assets/hourly-widget-template.html`](assets/hourly-widget-template.html) | [`references/hourly-widget.md`](references/hourly-widget.md) |
| Weekend widget | Friday night through Sunday night (five periods) | [`assets/weekend-widget-template.html`](assets/weekend-widget-template.html) | [`references/weekend-widget.md`](references/weekend-widget.md) |
| Long-weekend widget | Holiday or extra day off (seven or nine periods) | [`assets/long-weekend-widget-template.html`](assets/long-weekend-widget-template.html) | [`references/long-weekend-widget.md`](references/long-weekend-widget.md) |
| Week widget | Work week (Mon–Fri) or the next 7 days | [`assets/week-widget-template.html`](assets/week-widget-template.html) | [`references/week-widget.md`](references/week-widget.md) |
| Season card | Dates beyond the forecast range: 1991–2020 normals and records | [`assets/season-card-template.html`](assets/season-card-template.html) | [`references/season-card.md`](references/season-card.md) |

### Shared references

- [`references/data-sources.md`](references/data-sources.md): free, official data sources only (NWS API, NOAA centers, NCEI climate normals, international meteorological services, holiday calendars), plus tips for checking that data is fresh.
- [`references/icons.md`](references/icons.md): the Tabler outline icon and color for every condition. Color is reserved for hazards: red for warnings, amber for watches, silver for advisories.
- [`references/sample-data.md`](references/sample-data.md): the rules for demo widgets with made-up data.

## How an answer is shaped

A typical answer to "what's the weather?" has four parts:

1. The appropriate widget, with an alert banner above it if a watch or warning is active.
2. A few short bullets covering what the widget can't show at a glance.
3. One short paragraph on what the day will feel like.
4. A follow-up offering more detail, something interesting about the weather, or a longer look ahead.

## Data sources

Only free, official sources are used. No commercial weather APIs.

- **United States:** National Weather Service ([weather.gov](https://www.weather.gov), [api.weather.gov](https://api.weather.gov)) and other NOAA services.
- **Elsewhere:** the country's official national meteorological service, or the [WMO World Weather Information Service](https://worldweather.wmo.int).

## Installation

Copy this directory into your agent's skills folder (for example, `~/.claude/skills/weather-forecaster` or `~/.cursor/skills/weather-forecaster`). The agent loads `SKILL.md` when weather comes up and reads the referenced templates and specs as needed.

## License

[MIT](LICENSE)

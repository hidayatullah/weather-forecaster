# Weather Forecaster

**Version 0.0.1**

A Claude skill that turns Claude into a truthful, curious weather forecaster. It presents forecasts as consistent visual widgets, explains the science behind the weather, communicates watches and warnings clearly, and talks about seasons, climate patterns, weather history, and traditional forecasting.

It works in one-on-one chats and group channels. It answers direct questions right away, and politely offers the forecast when weather comes up in passing.

## Principles

- **Truthful above all.** Every number comes from an official source retrieved during the conversation. The forecaster never makes data up, never shades a forecast to please anyone, and says "I don't know" when it doesn't know.
- **Official, free sources only.** It uses the National Weather Service and NOAA in the US, and national meteorological services elsewhere. It never uses commercial weather APIs.
- **Report first, interpret second.** Any judgment of its own is clearly labeled, and comes with a reminder that conditions can change fast.
- **Made-up data only in explicit demos.** Sample data is allowed only when someone explicitly asks for a demo. It's always marked with a red "Sample data" ribbon, a "Not a forecast" caption, and a screen-reader note.
- **Color means danger.** Only watches (amber) and warnings (red) get color. Advisories and general notes are silver.

## Widgets

Every widget is built from a fixed HTML template, so it looks the same every time.

| Widget | When it's used |
|---|---|
| **Day card** | "What's the weather?" or "What's it like right now?": current conditions, humidity, dew point, and wind, plus the next two forecast periods |
| **Hourly widget** | "What's the weather going to be like?" or "When will the rain start?": the hour before, now, and the next four hours |
| **Weekend widget** | The weekend: Friday night through Sunday night |
| **Long-weekend widget** | Holiday weekends (detected from the public holiday calendar) or a personal day off, with the days off marked |
| **Work-week widget** | "This week," or Monday through Friday |
| **7-day widget** | "The week ahead," or the next 7 days |
| **Season card** | Any date beyond the forecast range: the 30-year averages (1991–2020 normals), records, and typical conditions for those dates |

Weather enthusiasts can also get one data chart after the widget, showing wind, precipitation, or temperature and dew point.

Each answer follows the same short format: the widget, a few bullets, one paragraph, and a follow-up question offering more detail, something interesting about today's weather, or a longer look ahead.

## Installation

### Claude app (claude.ai and desktop)

1. Download `weather-forecaster.skill` from the [Releases](../../releases) page.
2. In Claude, go to **Customize > Skills** and upload the file.
3. Ask Claude, "What's the weather?"

On Team and Enterprise plans, you can also share the skill with colleagues or publish it to your organization's skill library. See [Use skills in Claude](https://support.claude.com/en/articles/12512180-use-skills-in-claude).

### Claude Code

Copy the `weather-forecaster` skill folder into your personal skills folder (`~/.claude/skills/`), or into a project's `.claude/skills/` folder to share it with everyone who clones that repo.

> **Note:** The widgets are designed for the Claude app, where they render inline through Claude's visual tool. In Claude Code or through the API, the skill falls back to compact text versions of the same information.

## Skill structure

```
weather-forecaster/
├── SKILL.md                      Core instructions: persona, truthfulness, routing, answer format
├── references/
│   ├── data-sources.md           NWS/NOAA endpoints, international agencies, freshness checks, holiday calendars
│   ├── icons.md                  Condition icons and color families
│   ├── hourly-widget.md          Hourly widget spec and alert banners
│   ├── day-card.md               Day card spec
│   ├── weekend-widget.md         Weekend widget spec
│   ├── long-weekend-widget.md    Long-weekend widget spec
│   ├── week-widget.md            Work-week and 7-day widget spec
│   ├── season-card.md            Season card spec (30-year normals)
│   └── sample-data.md            Rules for clearly marked demo data
└── assets/
    ├── hourly-widget-template.html
    ├── day-card-template.html
    ├── weekend-widget-template.html
    ├── long-weekend-widget-template.html
    ├── week-widget-template.html
    └── season-card-template.html
```

## Data sources

- **Forecasts, observations, and alerts:** National Weather Service ([weather.gov](https://www.weather.gov), api.weather.gov)
- **Severe weather, hurricanes, and outlooks:** NOAA's Storm Prediction Center, National Hurricane Center, Weather Prediction Center, and Climate Prediction Center
- **Climate normals and records:** NWS office climate pages, NWS NOWData, and NCEI U.S. Climate Normals (1991–2020)
- **Outside the US:** each country's official national meteorological service, and the WMO World Weather Information Service

## Limitations

- Forecast pages fetched through web tools are sometimes served from a stale cache. The skill checks timestamps and shows "No data" rather than using old readings.
- Radar and satellite images can't be embedded in the chat, so the skill links to them instead.
- Free official coverage outside the US varies by country, so "I don't know" comes up more often there.
- This skill is not a substitute for official warnings. Always follow guidance from your local weather service and emergency officials.

## Contributing

Issues and pull requests are welcome. Please keep contributions in line with the skill's principles: official sources only, no invented data, and fixed templates for every widget.

## Changelog

### 0.0.1 (2026-09-26)

First public version: day card, hourly, weekend, long-weekend, work-week, and 7-day widgets; season card for dates beyond the forecast range; holiday detection; alert banners (warnings, watches, advisories); color-coded condition icons; weather enthusiast charts; and clearly marked sample data for demos.

## License

[MIT](LICENSE)

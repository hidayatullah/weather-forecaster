# Long-Weekend Widget Specification

The long-weekend widget is the weekend widget stretched to cover every day off, one NWS forecast period per column. It is marked with a silver calendar-star badge in the header, and each day off gets a gray star icon and a thin neutral outline. (Color is reserved for watches and warnings.)

It must look the same every time. Build it only from `assets/long-weekend-widget-template.html` and the exact snippets below. Fill in the data; don't change the layout, styling, class names, colors, icons, or wording patterns, and don't add or remove elements beyond what these rules allow. Every number must come from data retrieved in the current conversation, except in an explicitly requested demo (see `sample-data.md`).

The design was approved from live Round Rock, TX, Point Pleasant Beach, NJ, and Kahului, HI examples (three-day weekends) and a sample four-day weekend.

## Which periods

Start with the night before the first day off, and end with the night of the last day off:

| Days off | Columns | Per row |
|---|---|---|
| Sat–Mon (Monday off) | Fri night, Sat, Sat night, Sun, Sun night, Mon, Mon night (7) | 7 |
| Fri–Sun (Friday off) | Thu night, Fri, Fri night, Sat, Sat night, Sun, Sun night (7) | 7 |
| Fri–Mon (Friday and Monday off) | Thu night through Mon night (9) | 5 (wraps to two rows) |

Periods that have already passed stay as muted "Passed" columns (snippet in `weekend-widget.md`). The current period gets `now` in its class.

**When not to show the widget:** if the long weekend is beyond the NWS forecast range, so that most columns would be "No data", don't show the widget at all. Answer in text with the holiday line, the Climate Prediction Center outlooks that cover the dates, and typical conditions for that time of year (see SKILL.md, "Matching the time horizon"). Offer to check again once the forecast reaches the weekend. If only the last one or two periods are beyond range, show the widget and use the "No data" snippet for those.

## Placeholders

| Placeholder | Fill with |
|---|---|
| `{{COLUMNS_PER_ROW}}` | `7` for seven columns, `5` for nine. |
| `{{SR_SUMMARY}}` | "Long weekend forecast for Round Rock, TX, from Friday night through Monday night, September 25 to 28, 2026, with Monday off for Labor Day, from National Weather Service data." Start with "Sample data, not a real forecast:" in a demo. Add the alert clause when a banner is shown. |
| `{{SAMPLE_RIBBON}}` | Empty for real data. The ribbon snippet from `sample-data.md` in a demo. |
| `{{HEADER_PADDING}}` | Empty for real data. `;padding-right:70px` in a demo, so the ribbon doesn't cover the badge. |
| `{{LOCATION}}` | "City, ST" for US places. |
| `{{BADGE_TEXT}}` | "Long weekend · Sep 25–28", from the first night's date to the last day off. If a holiday caused it, it may name the holiday: "Labor Day weekend · Sep 4–7". |
| `{{BANNER}}` | A banner snippet from `hourly-widget.md` ("Banner snippets"): red for warnings, amber for watches, silver for advisories. Empty if there are no alerts. |
| `{{PERIOD_COLUMNS}}` | The period column snippets, in order. |
| `{{CAPTION}}` | "NWS Austin/San Antonio forecast, issued 1:24 PM CDT. <a href=\"{{SOURCE_URL}}\">View on weather.gov</a>" for real data, or "Sample data for illustration only. Not a forecast." in a demo. |

## Period column snippets

Use the weekend widget's column snippets (`weekend-widget.md`) with the same condition, icon, color, high or low, rain, and wind rules. Two differences:

**Labels:** "Thu night", "Fri", "Fri night", "Sat", "Sat night", "Sun", "Sun night", "Mon", "Mon night".

**Day off (daytime period of a holiday or other day off):** add `off` to the class and a star before the label:
```html
<div class="col off"><span class="t"><i class="ti ti-calendar-star" style="font-size:14px;color:var(--text-secondary)" aria-label="day off"></i>{{LABEL}}</span> ... </div>
```
If the day-off period is also the current period, use `class="col now off"`.

Weekend days themselves (Saturday, Sunday) are not marked; only the extra days off are.

## Reply text around the widget

Start with one short line saying why the weekend is long (see SKILL.md, "Long weekends and holidays"). Then follow the brief update format: a few bullets on alerts, the trend, and the best day for the person's plans, a short paragraph, and a follow-up question.

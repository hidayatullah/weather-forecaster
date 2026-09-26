# Week Widgets Specification (Work Week and 7 Days)

Two widgets share one template, `assets/week-widget-template.html`, with one column per day:

- **Work week:** exactly five columns, Monday through Friday.
- **7 days:** exactly seven columns, starting with today.

Both must look the same every time. Build them only from the template and the exact snippets below. Fill in the data; don't change the layout, styling, class names, colors, icons, or wording patterns, and don't add or remove elements beyond what these rules allow. Every number must come from data retrieved in the current conversation.

The design was approved from a live Round Rock, TX example.

## How to build it

1. Read `assets/week-widget-template.html`.
2. Replace every `{{PLACEHOLDER}}` using the rules below.
3. Render it with the inline visual tool (in claude.ai, the Visualizer's `show_widget`, with a title such as `round_rock_work_week_widget` or `round_rock_seven_day_widget`), passing the filled-in template exactly as it is.

## Which days

**Work week:**
- **Saturday or Sunday:** the coming Monday through Friday.
- **Monday through Friday:** the current Monday through Friday. Days that have already passed stay in place as muted "Passed" columns, so the widget always has five columns. Today gets the "now" highlight.

**7 days:** today plus the next six days. Today's column is labeled "Today" and gets the "now" highlight.

## Data source

The NWS 7-day forecast periods on the point forecast page (or `FcstType=dwml`). Each day combines two periods: the daytime period (conditions, high, wind, and daytime rain chance) and the following night period (low and night rain chance). If the question is asked in the evening, today's daytime period may be gone; then use "Tonight" for the low, and take today's conditions and high from the latest observation, or show "--" if neither is available.

## Placeholders

| Placeholder | Fill with |
|---|---|
| `{{COLUMN_COUNT}}` | `5` for the work week, `7` for 7 days. |
| `{{SR_SUMMARY}}` | "Work week forecast for Round Rock, TX, Monday September 28 through Friday October 2, 2026, from National Weather Service data." or "Seven-day forecast for ...". Add the alert clause when a banner is shown. |
| `{{LOCATION}}` | "City, ST" for US places. |
| `{{RANGE_LABEL}}` | "Work week" or "7 days". |
| `{{DATES}}` | First through last date shown: "Sep 28–Oct 2". |
| `{{BANNER}}` | A banner snippet from `hourly-widget.md` ("Banner snippets"): red for warnings, amber for watches, silver for advisories. Empty string if there are no alerts. |
| `{{DAY_COLUMNS}}` | Five or seven day-column snippets, in order. |
| `{{CAPTION}}` | "NWS Austin/San Antonio forecast, issued 1:24 PM CDT." |
| `{{SOURCE_URL}}` | The weather.gov point forecast page. |

## Day column snippets

**Forecast day:**
```html
<div class="col"><span class="t">{{DAY}}</span><span class="tag">{{CONDITION}}</span><i class="ti {{ICON}} ic" style="color:{{ICON_COLOR}}" aria-hidden="true"></i><span class="temp">{{HIGH}}°F</span><span class="gust">Low {{LOW}}°F{{RAIN}}</span><span class="wind">{{WIND}}</span></div>
```
- `{{DAY}}`: "Mon", "Tue", and so on, or "Today" for today in the 7-day widget. Today uses `class="col now"`.
- `{{CONDITION}}`: a short version of the daytime short forecast, in sentence case, 2–5 words: "Sunny", "Partly sunny", "Showers, t-storm possible", "Mostly sunny, late showers".
- `{{ICON}}` and `{{ICON_COLOR}}`: from `icons.md`, based on the daytime period.
- `{{HIGH}}`, `{{LOW}}`: from the day and night periods. If either is missing (for example, the last night is past the forecast range), write `--` without "°F": "Low --".
- `{{RAIN}}`:
  - If the daytime chance is 20% or more: ` · 50% rain`.
  - Otherwise, if only the night chance is 20% or more: ` · 30% rain at night`.
  - Otherwise: empty.
- `{{WIND}}`: the daytime wind, using the same wind snippets as the weekend widget (see `weekend-widget.md`, "Wind"): arrow plus compass label, speed range, ", gusts N" when the NWS mentions gusts, "X, then Y" for shifting wind, `Calm`, or `--` when the forecast gives no wind.

**Passed day** (work week only):
```html
<div class="col"><span class="t">{{DAY}}</span><span class="tag">Passed</span><i class="ti ti-clock ic" style="color:var(--text-muted)" aria-hidden="true"></i><span class="temp" style="color:var(--text-muted)">--</span><span class="wind">--</span></div>
```

**Beyond the forecast range:** use the "No data" column snippet from `hourly-widget.md`, with the day label.

## Reply text around the widget

Follow the brief update format in SKILL.md: a few bullets for the trend and what the widget doesn't show (for example, "turning stormy midweek", which days any alerts cover, or rain timing within a day), a short paragraph, and a follow-up question. Forecast confidence drops after about five days, so say so briefly when the widget reaches that far.

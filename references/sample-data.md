# Sample (Made-Up) Data

Truthfulness is the core of this skill, so made-up weather data is allowed in exactly one situation: the person explicitly asks for a demo, example, mockup, or test layout (for example, "show off the widget with made-up data"). It is never used in a real weather answer, never used to fill gaps in real data, and never mixed with real data in the same widget.

Every widget with made-up data must carry all three markers:

1. **A diagonal red ribbon** with white text reading "Sample data" in the top-right corner:
   ```html
   <div class="ribbon" aria-hidden="true">Sample data</div>
   ```
   with this rule added to the widget's `<style>` block (the long-weekend template already includes it) (the widget wrapper must have `position:relative;overflow:hidden`):
   ```css
   .ribbon{position:absolute;top:22px;right:-52px;transform:rotate(35deg);background:#A32D2D;color:#fff;font-size:12px;font-weight:500;letter-spacing:.02em;padding:5px 60px;z-index:2;pointer-events:none}
   ```
   Add `padding-right:70px` to the header row so the ribbon doesn't cover the header content.
2. **The caption** "Sample data for illustration only. Not a forecast." in place of the source line. There is no weather.gov link.
3. **The screen-reader summary** starting with "Sample data, not a real forecast:".

Sample data should be realistic for the place and season, and should be internally consistent (lows below highs, wind shifts that make sense). The reply text should also say plainly that the data is made up.

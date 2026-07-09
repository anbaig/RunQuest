# Reward shop for wife

_Started 2026-07-08 23:40 UTC_

---

## User

I am trying to design a web app for my wife that lets me reward her for doing things that are good for her with points that she can use to redeem in a shop full of prizes that I have put there -- when she buys things in the shop I will actually buy them for her

## Assistant

_[tool: list_files]_

_[tool: list_files]_

_[tool: questions_v2]_

## User

Questions answered:
- app_name: give me some suggestions
- roles: Both of us in one app — I have a hidden/separate 'admin' mode to add points and manage the shop, she has her own view
- earning_actions: currently the only thing she can do is run for a certain set of miles and the points correlate with miles -- something like 1 point is 1 mile and if she goes on a streak, 1 mile starts being the equivalent of more than one point.
- prizes: Lego sets, eating out, going to barnes and nobles, just fun things but yeah put placeholders as ill be putting things in there as they come up
- tone: something like an 8 bit/18 bit game in the style of undertale/deltarune -- I want it to feel like a game
- platform: Phone (mobile web app)
- screens: Just the Dashboard + Shop to start
- variations: One strong direction, refine from there

## User

Found issues — fixing…

## User

Found issues — fixing…

## Assistant

_[tool: dc_html_str_replace]_

_[tool: dc_html_str_replace]_

_[tool: ready_for_verification]_

Increased footer clearance so the streak card and run log aren't hidden behind the sticky button.

## User

I don't really get the point of the level system?

## Assistant

The level (LV) badge is just a lifetime-progress framing on top of raw points — it never decreases when she spends points, so it shows long-term commitment even after she cashes out a reward, and gives a "leveling up" moment as a bonus dopamine hit alongside the shop. If it feels redundant with the points balance, I can drop it and simplify the dashboard, or replace it with something more meaningful — want me to cut it, or swap it for something else (e.g. a badge/milestone system tied to actual streak achievements)?

## User

I like it but I think instead of a heart it should be an avatar that levels up

## Assistant

_[tool: dc_html_str_replace]_

Now the logic to build the tiered pixel avatar.

_[tool: dc_js_str_replace]_

_[tool: dc_js_str_replace]_

_[tool: dc_js_str_replace]_

_[tool: ready_for_verification]_

Swapped the heart for a pixel-art avatar that gains a headband, cape, and crown at LV 3/6/10, with a title (ROOKIE → RUNNER → CHAMPION → LEGEND).

## User

there isn't enough contrast between the avatar and the background so they kinda just blend together :(

## Assistant

_[tool: dc_html_str_replace]_

_[tool: dc_js_str_replace]_

_[tool: ready_for_verification]_

Added a dark backing panel behind the avatar and lightened the hair/pants colors so the sprite pops against the card.

## User

perfect -- the only thing missing is the ability to log a run that's a normal user operation

## Assistant

_[tool: dc_html_str_replace]_

_[tool: dc_js_str_replace]_

Add default `showLogRun: false` state and expose the new handlers/styles in renderVals.

_[tool: dc_js_str_replace]_

_[tool: dc_js_str_replace]_

_[tool: read_file]_

_[tool: grep]_

Good — `previewPoints`/`awardDisabled` are defined at line 516-517, before line 545 where they're used. No TDZ issue. Let's verify.

_[tool: ready_for_verification]_

Added a "LOG A RUN" button on her dashboard (no PIN needed) that opens the same run-logging form, so she can self-report miles directly.

## User

this works very well -- I want to actually implement it for real!

## Assistant

_[tool: questions_v2]_

## User

Questions answered:
- device_setup: She's not incredibly technical so I can make it so that I can login to the GM portal and pass a config file that populates the app with new data -- so I guess we can share. But state between visits needs to persist for her

## Assistant

_[tool: dc_html_str_replace]_

_[tool: dc_html_str_replace]_

Now add the SYNC tab in GM mode and wire up all the logic.

_[tool: dc_html_str_replace]_

_[tool: dc_html_str_replace]_

Now the logic: encode/decode helpers, handlers, state fields, and renderVals wiring.

_[tool: dc_js_str_replace]_

_[tool: dc_js_str_replace]_

_[tool: dc_js_str_replace]_

_[tool: dc_js_str_replace]_

_[tool: dc_js_str_replace]_

Also tighten the GM tab row font since it now has 4 tabs.

_[tool: dc_html_str_replace]_

_[tool: dc_js_str_replace]_

_[tool: ready_for_verification]_

Added a Sync system: a ⇄ button on her dashboard and a SYNC tab in GM mode. Each side shows a "your code" text block to copy and send (text/AirDrop), plus a paste box to load an update — so you can text each other a code after logging a run or after she buys something in the shop, and both phones stay in sync. Next: once you're happy, I can save this as a standalone offline HTML file you both add to your home screens.


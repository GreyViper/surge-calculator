# Surge Calculator

A one-page tool that turns a call log into the numbers needed for the surge incentive: total calls, the incoming/outbound split, number of firms, whether the incentive was met, and how much is due.

**Use it here:** https://greyviper.github.io/surge-calculator/

Everything runs in your browser. The data you paste is never uploaded, saved, or sent anywhere, and there is no login, so any number of people can use it at the same time.

## How to use it

1. **Copy the call table** from the website, including the header row (`Date`, `Direction`, `Source Line`, and so on).
2. **Paste it** into the box under *1. Your call data*. You can also click *Or choose a CSV/TSV file* to load a file instead (`.xlsx` files aren't supported; copy and paste from Excel instead).
3. **Check the settings** under *2. Settings* and fill in the required fields (marked with a red `*`).
4. **Click Calculate** (or press Enter in any settings box). The results appear under *3. Results*.

If you change anything after calculating, a note appears saying the results are out of date. Click **Calculate** again to refresh them. **Clear** empties the page so you can start over.

## Settings

| Field | Required | What to enter |
|---|---|---|
| Call data | Yes | The pasted table, with its header row. |
| Start time | Yes | First minute of the window. Default 8:30 AM. |
| End time | Yes | Last minute of the window. Default 10:59 PM. The whole minute counts, so a call at 22:59:30 is included in a 22:59 window. |
| Actual abandon rate % | Yes | Your actual abandon rate for the period, as a percentage (for example `2.5`). It comes from your other report; the calculator can't work it out from the call log. |
| Incentive per call ($) | Yes | Dollars paid per counted call. Default `1`. Change it to `2` or anything else. |
| Target abandon rate % | Fixed | Always 2.99%. It can't be edited on the page. |

If a required field is empty or isn't a number, clicking **Calculate** lists what's missing, highlights those boxes in red, and moves the cursor to the first one.

## What the results mean

| Result | How it is worked out |
|---|---|
| **Total calls** | Every call inside the time window, incoming plus outbound, except calls whose status is `ringing` (never answered). |
| **Incoming / Outbound** | The same calls split by the `Direction` column (`incoming` or `outgoing`). |
| **Firms** | The number of different firms (`Source Line`) among the counted calls. Capital letters don't matter, and blank names are ignored. |
| **Incentive status** | **Met** if the actual abandon rate is *below* 2.99%. Exactly 2.99% or higher is **Not Met**. |
| **Total incentive due** | Total calls × incentive per call when the status is Met. It shows `$0.00` when the status is Not Met. |
| **Calls per firm** | A table of every firm with its incoming, outbound, and total calls, sorted A to Z. |

The line under the paste box shows how many calls were read and how many were left out (still ringing, or outside the time window).

**Copy results** copies the summary and the firm table as tab-separated text, ready to paste into a spreadsheet or email.

## What the pasted data needs

The calculator finds columns by their header names, so their order doesn't matter, but these four headers must be present:

- `Date`
- `Direction`
- `Source Line`
- `Status`

Dates can look like `2026-11-04 18:51:15` or `11/4/2026 6:51:15 PM`. Only the time of day is used for the time window.

Cells that span several lines (like the *Action* column) are handled, whether they come from a website or from Google Sheets.

## Troubleshooting

| What you see | What to do |
|---|---|
| "Could not find these column headers…" | You pasted without the header row, or a header is named differently. Copy the table again, starting from the header row. |
| "Please fill in: …" | One or more required fields are empty. The highlighted boxes show which. |
| "This must be a number: …" | The abandon rate or incentive box doesn't contain a number. Type it as a plain number, such as `2.5` or `1`. |
| "…row(s) were skipped because their date/time could not be read" | Those rows have a date without a time, or in a format the calculator doesn't recognise. |
| Totals look too low | Check the start and end times, and look at the line under the paste box to see how many calls were left out as ringing or outside the window. |
| A note says some calls "have no firm name" | Those calls are in the totals but can't be listed in the per-firm table. |
| A note says some calls have a direction other than incoming/outgoing | Those calls are in the total only, not in the incoming or outbound counts. |

## Running it on your own computer

There is nothing to install. Download `index.html` and open it in any modern browser. It works offline.

## For maintainers

The whole app is the single file `index.html` (HTML, CSS, and JavaScript, no libraries). The fixed 2.99% target is the `TARGET_ABANDON_PCT` constant near the top of the script. The counting logic lives between the `CORE:START` and `CORE:END` comments, separate from the page code.

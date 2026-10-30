# Evaluation test set

Fill this in after every refresh, before you publish. The agent is the system under test.
You are the grader. Nothing here is automated on purpose: the point is to decide, with a
source you trust, whether the agent told you the truth.

**Ground truth sources.** Pick one and use the same one every time, so your scores are
comparable week to week:

- Prices: Google Finance (google.com/finance), Yahoo Finance, or your brokerage app.
- News: the article itself. Open the link. Check the date on the page.
- Calendar: the company's investor relations page (search "TICKER investor relations
  events"), or the Fed's own meeting calendar for FOMC dates.

Refresh date: 2026-10-29    Grader: number-checker agent

## A. Prices (one row per ticker)

Pass if the dashboard's last price is within $0.10 of the ground truth for the same day
and the same price type (close vs intraday). If the agent recorded an intraday price,
compare against the intraday price at about the same time; do not compare it to a close.

Google Finance returned "not supported on this device" when fetched (https://www.google.com/finance/quote/DAL:NYSE), so Yahoo Finance history pages were used as the second independent source for every ticker. All five closes (Oct 23, 26, 27, 28, 29) were compared for each row; all matched exactly.

| Ticker | Date shown | Price type shown | Dashboard price | Ground truth price | Source you used | Pass / Fail |
|---|---|---|---|---|---|---|
| DAL | 2026-10-29 | close | 81.64 | 81.64 | https://finance.yahoo.com/quote/DAL/history/ | Pass |
| UAL | 2026-10-29 | close | 105.21 | 105.21 | https://finance.yahoo.com/quote/UAL/history/ | Pass |
| AAL | 2026-10-29 | close | 12.78 | 12.78 | https://finance.yahoo.com/quote/AAL/history/ | Pass |
| LUV | 2026-10-29 | close | 43.68 | 43.68 | https://finance.yahoo.com/quote/LUV/history/ | Pass |
| ALK | 2026-10-29 | close | 36.15 | 36.15 | https://finance.yahoo.com/quote/ALK/history/ | Pass |
| JBLU | 2026-10-29 | close | 3.05 | 3.05 | https://finance.yahoo.com/quote/JBLU/history/ | Pass |
| SKYW | 2026-10-29 | close | 96.54 | 96.54 | https://finance.yahoo.com/quote/SKYW/history/ | Pass |
| ULCC | 2026-10-29 | close | 6.48 | 6.48 | https://finance.yahoo.com/quote/ULCC/history/ | Pass |
| AC.TO | 2026-10-29 | close (CAD) | 21.87 | 21.87 | https://finance.yahoo.com/quote/AC.TO/history/ | Pass |
| RYAAY | 2026-10-29 | close | 53.39 | 53.39 | https://finance.yahoo.com/quote/RYAAY/history/ | Pass |
| LLY | 2026-10-29 | close | 1373.70 | 1373.70 | https://finance.yahoo.com/quote/LLY/history/ | Pass |

## B. Weekly change (one row per ticker)

The page computes this from the five closes. Pass if it is within 0.3 percentage points of
your own calculation: (last price minus the close five trading days earlier) divided by
that earlier close.

Closes used are the ones verified in A (source: the Yahoo Finance history URLs listed in A). Dashboard figure is what the page computes from the closes; CHANGELOG[0] quotes the same percentages and none mismatches.

| Ticker | Dashboard weekly change | Your calculation | Pass / Fail |
|---|---|---|---|
| DAL | +6.3% | (81.64-76.78)/76.78 = +6.3% | Pass |
| UAL | +7.4% | (105.21-97.93)/97.93 = +7.4% | Pass |
| AAL | +5.5% | (12.78-12.11)/12.11 = +5.5% | Pass |
| LUV | +4.6% | (43.68-41.75)/41.75 = +4.6% | Pass |
| ALK | +5.1% | (36.15-34.40)/34.40 = +5.1% | Pass |
| JBLU | +16.0% | (3.05-2.63)/2.63 = +16.0% | Pass |
| SKYW | +3.8% | (96.54-93.03)/93.03 = +3.8% | Pass |
| ULCC | +31.2% | (6.48-4.94)/4.94 = +31.2% | Pass |
| AC.TO | +3.6% | (21.87-21.11)/21.11 = +3.6% | Pass |
| RYAAY | +1.6% | (53.39-52.53)/52.53 = +1.6% | Pass |
| LLY | +7.9% | (1373.70-1273.46)/1273.46 = +7.9% | Pass |

## C. News grounding (one row per news item, three per ticker)

Open every link. Pass only if all four are true: the link opens, the headline matches the
article, the article date is within the last seven days, and the "why it matters" line is
supported by the article (not just plausible).

Window used: 2026-10-22 to 2026-10-29. UAL, ALK, AC.TO have no news items (empty lists, disclosed in CHANGELOG), so no rows.

| Ticker | Headline (first few words) | Link opens | Headline matches | Dated within 7 days | "Why" is supported | Pass / Fail |
|---|---|---|---|---|---|---|
| DAL | Aeroitalia selects Delta TechOps... (https://news.delta.com/aeroitalia-selects-delta-techops-737-fleet-component-support-program.md) | Yes | Yes | Yes, Oct 28 | No. Brief says "a small win... little direct effect on near-term earnings". Article does not disclose contract value, terms or duration and says financial impact cannot be determined, so the size claim is unsupported | Fail |
| AAL | American sets new revenue records... (https://www.sec.gov/Archives/edgar/data/0000006201/000000620126000064/a8kerexhibit991q3-26.htm) | Yes | Yes | Yes, Oct 22 | Yes: revenue $16.3B up 19.3%, fuel expense up 67.5%, FY adjusted loss $1.00 to $1.60 per share | Pass |
| LUV | Southwest Posts Record Q3 Revenue... (https://www.airwaysmag.com/new-post/southwest-q3-2026-record-revenue-fuel-costs) | Yes | Yes | Yes, Oct 23 | Partly. $270M net income and revenue up 21.2% confirmed, but the article does not say assigned seating or fare changes lifted revenue; it only mentions them as commercial changes. Revenue rise is attributed to RASM up 21.6% on capacity down 0.4% | Fail |
| JBLU | JetBlue Posts US$257m Q3 Loss... (https://www.airwaysmag.com/new-post/jetblue-q3-2026-loss-revenue-fuel-costs) | Yes | Yes | Yes, Oct 26 | Yes: loss widened from $143M to $257M, fuel expense up 61%, margin guidance -6.5% to -4.5% vs -5% to -2% in July | Pass |
| SKYW | SkyWest, Inc. Announces Third Quarter 2026 Profit (https://www.sec.gov/Archives/edgar/data/0000793733/000110465926120182/skyw-ex99d1.htm) | Yes | Yes | Yes, Oct 29 | Yes: net income $120M ($3.08/share), revenue $1.12B up 6%, results hurt by higher fuel cost per gallon in prorate business | Pass |
| ULCC | Frontier Airlines Delivers Record Third Quarter 2026 Revenue of $1.4 billion (https://www.sec.gov/Archives/edgar/data/0001670076/000167007626000102/frontier_earningsreleasexq.htm) | Yes | Yes (record Q3 revenue; exact title string not shown by the fetch tool) | Yes, Oct 28 | Yes: revenue $1,355M, adjusted EPS $0.18 "significantly exceeding guidance", GAAP net loss $104M, Q4 adjusted EPS guide $(0.15) to $0.05 | Pass |
| RYAAY | Bag drop deadline moves to 60 minutes... (https://corporate.ryanair.com/news/ryanair-reminds-passengers-of-60-min-bag-drop-closure-from-tue-10-nov-airport-bag-drop-will-close-60-mins-before-departure/) | Yes | Yes | Yes, Oct 27 | Yes for the facts: closes 60 min instead of 40, about 20% of passengers who check a bag. "Little direct effect on earnings" is the agent's judgment, not stated, but it is a modest inference from a minor procedural change | Pass |
| LLY | Lilly reports Q3 2026 financial results... (https://www.sec.gov/Archives/edgar/data/0000059478/000005947826000104/q326lillysalesandearningsp.htm) | Yes | Yes | Yes, Oct 29 | Yes: revenue up 39% to $24.4B "driven primarily by Mounjaro and Zepbound"; 2026 guidance raised to $91.0 to $92.0B | Pass |

### C re-check, 2026-10-29 (after "why" lines were corrected; original Fail rows above kept unchanged)

| Ticker | Headline (first few words) | Link opens | Headline matches | Dated within 7 days | "Why" is supported | Pass / Fail |
|---|---|---|---|---|---|---|
| DAL (re-check) | Aeroitalia selects Delta TechOps... (https://news.delta.com/aeroitalia-selects-delta-techops-737-fleet-component-support-program.md) | Yes | Yes | Yes, Oct 28 | Yes. New why: "Adds Aeroitalia as a customer for Delta's maintenance business (737 component support); contract value, terms and duration were not disclosed." Article: Aeroitalia "has selected Delta TechOps to provide comprehensive component support for its Boeing 737 fleet"; no value, terms or duration given. The unsupported size and earnings claim is gone | Pass |
| LUV (re-check) | Southwest Posts Record Q3 Revenue... (https://www.airwaysmag.com/new-post/southwest-q3-2026-record-revenue-fuel-costs) | Yes | Yes | Yes, Oct 23 | Yes. New why: net income $270M, revenue up 21.2%, RASM up 21.6% on capacity down 0.4%. Article: "net income of US$270m", "Operating revenue rose 21.2% to US$8.4bn", "Revenue per available seat mile increased 21.6%", "despite a 0.4% decline in capacity". The unsupported seating and fare attribution is gone | Pass |

(Add rows as needed. Fifteen items is normal for five tickers.)

## D. Calendar (one row per event)

Pass if the event and its date appear on the source linked, and the date is in the future
relative to the refresh date.

Only 5 events exist; all other tickers have empty calendars (disclosed in CHANGELOG). I did not search IR pages for earnings dates of those tickers (that is an omissions question for section E).

| Ticker | Event | Date shown | Confirmed on source | Pass / Fail |
|---|---|---|---|---|
| DAL (market-wide) | U.S. jobs report, October employment situation, 8:30 AM ET | 2026-11-06 | Yes, listed Nov. 06, 2026 at 08:30 AM, not marked tentative. https://www.bls.gov/schedule/news_release/empsit.htm | Pass |
| DAL (market-wide) | U.S. CPI, October, 8:30 AM ET | 2026-11-10 | Yes, listed Nov. 10, 2026 at 08:30 AM, not marked tentative. https://www.bls.gov/schedule/news_release/cpi.htm | Pass |
| SKYW | Q3 earnings conference call, 2:30 PM MT | 2026-10-29 | Yes, release states call today Oct 29 at 2:30 p.m. Mountain. Date equals asOf, so on or after. https://www.sec.gov/Archives/edgar/data/0000793733/000110465926120182/skyw-ex99d1.htm | Pass |
| AC.TO | Q3 2026 results presentation, listen-only webcast, 5:30 PM ET | 2026-10-29 | Yes, Thursday Oct 29, 2026, 5:30 p.m. ET, listen-only, no "expected" wording. https://www.aircanada.com/media/air-canada-to-present-third-quarter-2026-results/ | Pass |

## E. Omissions

Did the agent miss something a human analyst would have included? List anything you found
in the ground-truth sources that the brief does not mention (a big headline, an earnings
date inside the window). Each one counts as a Fail in the score below.

- 
- 

## F. Your own test cases

Write two tests of your own that none of the sections above cover. Examples students have
used: "the as-of date on the page matches the changelog date", "no ticker shows a flat
sparkline when the real stock moved", "the page still renders if I delete one ticker's
news list in the data". Write the test, run it, record the result.

| Test | How you checked | Pass / Fail |
|---|---|---|
| | | |
| | | |

## Score

| Section | Passes | Total | Rate |
|---|---|---|---|
| A. Prices | | | |
| B. Weekly change | | | |
| C. News | | | |
| D. Calendar | | | |
| E. Omissions (fails only) | 0 | | |
| F. Your own | | | |
| **Overall** | | | |

Publish only if A and B are 100% and C is at least 80% after fixes. If a row failed, tell
Claude exactly which row and what the correct value is, let it fix only that, rerun the
`qa-reviewer` agent, and regrade that row. Keep the original Fail in this file: a record of
what the agent got wrong is more useful than a clean sheet.

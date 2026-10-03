# Insulation V: demo website

Demo site for the CUHK OSALP 2025-04 capstone (team Insulation V): parametric micro-insurance for concert trips.

- With the AI underwriter: https://insurnce.github.io/insulation-v/
- Without AI (built-in products only): https://insurnce.github.io/insulation-v/offline.html

The site is one static page (`index.html`) plus a free Cloudflare Worker (`worker/worker.js`) that runs the AI underwriter with an open-source Llama model. No login and no API key are needed.

## How a request is decided
Five insurability tests: insurable interest, pure risk, outside your control, low and measurable probability, within our scope. A risk must pass all five.

**No data, no price.** Every probability comes from published data. The AI may only pick a row from the data table in `worker.js`; the code does all the arithmetic. If no row measures the event, the site says "We can't price this risk yet" and names the data it would need.

| Data row | Rate | Source |
|---|---|---|
| Theft of personal property | 10,920 ÷ 7,527,500 = 0.15% per person per year | HK Police, Police in Figures 2024 and 2025; C&SD mid-2025 population |
| Pickpocketing or snatching | 257 ÷ 7,527,500 = 0.0034% per person per year | same |
| Home burglary | 816 ÷ 2,775,300 = 0.029% per household per year | HK Police; C&SD General Household Survey Q2 2025 |
| Typhoon Signal 8+ on a given day | 76 ÷ 9,497 = 0.80% per day | HKO Warnings & Signals Database, 2000–2025 |
| Black Rain or Signal 8+, 09:00–18:00 | 61 ÷ 9,497 = 0.64% per day | same |
| Black Rain or Signal 8+, 17:00–23:00 | 47 ÷ 9,497 = 0.495% per evening | same |
| Delayed checked bag | 6.3 ÷ 1,000 × 74% = 0.47% per flight | SITA Baggage IT Insights 2025 and 2026 |
| Phone screen damage | 31% × 67% = 20.8% per year (declined: above 20%) | Allstate Protection Plans surveys 2023 (2,504 US adults) |

Concert Trip Cover: 0.495% weather (HKO) + 1% other causes (documented 4 of about 522 large shows, Jan 2024–Sep 2026 = 0.77%, rounded up) = 1.49% per concert.

## Fixed answers
- "cannot get concert tickets": declined (fails all five tests)
- "have concert tickets but concert postponed" (or "cancelled"): approved, Concert Trip Cover (premium HK$25, deductible HK$150, co-pay 20%, limit HK$960)
- Built-in products when the AI is unavailable: typhoon travel, delayed baggage, theft, burglary, rain-out
- Built-in declines: gambling and investments, exams, losses that already happened, phone screens (too likely), lost items (no data)

## Feedback response
See `changes.html` (linked in the page footer): each teacher comment and what changed.

## What the page shows
- **Explain the decision step by step**: what would be insured, then each test as question, rule, why the rule exists, your request and result, then the decision, what would go wrong if we covered it anyway, and what we can cover instead.
- **How did we get this chance?**: the data, the rate and the conversion to your policy period, with the year-to-year range.
- **Premium breakdown**: how much of each premium pays expected claims and how much covers costs, capital and margin.
- **Seasonal pricing**: weather covers are priced from HKO counts for the month of the start date (e.g. Typhoon Travel Cover HK$10 in January, HK$125 in September). Concert Trip Cover keeps a flat HK$25 and shows the seasonal price for comparison.
- **Live HKO check**: weather covers read the Observatory's open warning feed and pause sales while a tropical cyclone signal or rainstorm warning is in force.
- **Run the simulation** (on the quote and on the policy):
  1. Roll the dice once, 100 or 1,000 times, with running totals of premiums and payouts.
  2. The whole pool 10,000 times, drawn with Plotly, with sliders (concert: other-cause rate and number of concerts; other products: pool size and "what if the real chance is ×0.5–×3"). Concert Cover mirrors the R model and gives the same 6.4% losing years, HK$234,500 capital, and about 28% losing years at a 2% other-cause rate.
  3. Reality check: what each product would have paid in every real year of its data, with named storms for HKO covers (47 concert-hours storm evenings 2000–2025, e.g. Mangkhut 2018) and a busy-night stress test for concerts.
- The AI worker saves answers for 24 hours, so repeated questions are instant and free (KV namespace `CACHE` if bound, otherwise memory).

## Testing
`tests/regression.js`: paste into the browser console on the page; 20 checks covering the teacher's cases, the R match, seasonal prices and the live HKO pause.

Chris Wong and the band "The Midnight Echoes" are fictional.

## Publish
See SETUP.md.

AI underwriter built with Llama.

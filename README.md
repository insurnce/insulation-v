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

## What the page shows
- **Explain the decision step by step**: what would be insured, then each test as question, rule, why the rule exists, your request and result, then the decision, what would go wrong if we covered it anyway, and what we can cover instead.
- **How did we get this chance?**: the data, the rate and the conversion to your policy period, with the year-to-year range.
- **Run the simulation** (on the quote and on the policy): roll real random numbers for one customer, then run the whole pool 10,000 times, drawn with Plotly. One-line model plus every parameter and its source. Concert Cover mirrors the R model (Poisson storm nights, Binomial other cancellations) and gives the same 6.4% losing years and HK$234,500 capital. Other products replay the real years in their data.

Chris Wong and the band "The Midnight Echoes" are fictional.

## Publish
See SETUP.md.

AI underwriter built with Llama.

# Insulation V: demo website

Demo site for the CUHK OSALP 2025-04 capstone (team Insulation V): parametric micro-insurance for concert trips.

The site is one static page (`index.html`) plus a free Cloudflare Worker (`worker/worker.js`) that runs the AI underwriter with an open-source Llama model. No login and no API key are needed. Fixed answers:

- "cannot get concert tickets": declined (fails all four insurability tests)
- "have concert tickets but concert postponed" (or "cancelled"): approved, Concert Trip Cover for train, hotel and booking fees (premium HK$25, deductible HK$150, co-pay 20%, limit HK$960)
- Backup products when the AI is unavailable: flight delay, delayed baggage, phone screen, rain-out
- Backup declines: gambling and investments, exams, losses that already happened
- Every other request goes to the AI underwriter, which applies the four insurability tests and designs and prices a policy.

Chris Wong and the band "The Midnight Echoes" are fictional. Probabilities for products other than the concert cover are illustrative.

## Publish
See SETUP.md.

AI underwriter built with Llama.

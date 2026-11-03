# When the Network Wears Someone Else's Brand

**A wholesale CX case study of the Aussie Broadband Group, built entirely from public data.**

How wholesale partners experience the Aussie Broadband Group, what the public data says about it, and what I would do in my first 90 days as a Graduate CX Coordinator.

> [!NOTE]
> This is an independent portfolio project. It uses public data only and is not affiliated with, endorsed by, or produced for Aussie Broadband or Symbio.

## The problem

Aussie Broadband's Wholesale business ($297m revenue in FY26) lets partners resell the Group's network under their own brand. Their customers rarely see the Aussie or Symbio name, so when a service drops or a fix is slow, it is the **partner's** brand that takes the hit. Wholesale CX is a two-layer problem: it has to serve the partner's team *and* protect their customers' experience.

| Signal | Figure |
| --- | --- |
| Growth in wholesale broadband connections | **546%**, to 336k |
| Share of those connections from a single migration | **80%** (269k services) |
| Ombudsman (TIO) complaints, Oct–Dec 2025 vs prior quarter | **+49%** (industry: +3.5%) |
| Published partner satisfaction scores, across 1,000+ partners | **0** |

## What the data shows

**1. Same company, very different scores.** Public ratings range from 1.8/5 (Trustpilot, 321 reviews) to 4.4/5 (ProductReview, 10,279 reviews), with independent 2026 surveys in between (WhistleOut 4.19, Finder 4.10, CHOICE 80% satisfied). Reviews are useful for spotting themes but can't tell you how a specific partner feels.

**2. Complaints peaked during the big migration.** TIO complaints hit 302 in Oct–Dec 2025, the peak quarter, while about 269,000 services were being migrated. This is timing, not proven cause, and it's the first thing I'd test against internal ticket data.

**3. Reliability is the most criticised theme; communication is the most praised.** Across 312 Trustpilot reviews (Dec 2025–Oct 2026) coded against 8 CX themes, complaints centre on outages, long holds and escalations, and rarely on the agents themselves.

## Four insights and recommendations

| # | Insight | Recommendation |
| --- | --- | --- |
| 1 | People are the strength; reaching them is the problem. | **Give partner escalations their own lane:** a published callback and escalation service level, with proactive fault updates. |
| 2 | Migrations are the moment of truth. | **Add CX checkpoints to every migration:** record pre-cutover speeds, give partners a customer comms kit, run day 7 and day 30 pulses. |
| 3 | Price changes travel down the chain. | **Send partners a price-change kit:** a fixed minimum notice period plus a rebrandable FAQ, email templates and retention tips. |
| 4 | Public reviews can't be the listening system. | **Launch a partner listening plan:** relationship survey and pulses, close the loop with every detractor, report a monthly CX scorecard. |

The deck also includes a draft five-stage partner journey map (Evaluate → Integrate → Migrate → Operate → Renew) with friction points, listening touchpoints and a metric for each stage, plus a 30/60/90-day plan.

## The notebook

[`CX_Case_Study_Analysis.ipynb`](CX_Case_Study_Analysis.ipynb) backs up every number in the deck and runs top to bottom in Google Colab. Every input is typed in with its source and page number so it can be checked by hand.

| Section | What it does | Tools |
| --- | --- | --- |
| 1 · Headline numbers | Recalculates the slide 2 figures from the Annual Report and TIO reports | pandas |
| 2 · Ombudsman trend | Charts quarterly TIO complaints, using revised figures where the TIO restated them | matplotlib |
| 3 · Review themes | Splits Trustpilot reviews into sentences, tags each against 8 CX themes, scores tone, and charts complaints against praise | `transformers` zero-shot (`facebook/bart-large-mnli`), VADER |
| 4 · Survey export | Writes the relationship survey and three pulses as a Qualtrics Advanced Format import file | Python |
| 5 · Scorecard | NPS, CSAT, effort score and detractor-callback functions for the monthly partner scorecard | pandas |

```python
# Section 1: headline numbers
share_from_migration = 269_000 / 336_000   # 80%
abb_qoq              = 302 / 203 - 1       # +49%
industry_qoq         = 14_017 / 13_547 - 1 # +3.5%
```

In section 3, the AI model does the first pass: a theme is tagged when its confidence is at least 0.60, and VADER sets the tone (with the star rating breaking ties when a sentence reads as neutral). Every tag is exported to `review_tags_for_checking.csv` with a blank `checked_by_hand` column, so a person makes the final call. A final cell compares the AI's counts with the hand-coded counts.

> [!IMPORTANT]
> Retail customer reviews are used as a **proxy** for wholesale experience, because no partner-level satisfaction data is published. Findings are hypotheses to test with partner interviews and internal ticket data, not conclusions.

### Running it

1. Open the notebook in [Google Colab](https://colab.research.google.com/). Sections 1, 2, 4 and 5 run as they are.
2. For section 3, upload a `trustpilot_reviews.csv` with the columns `date`, `rating` (1–5) and `text` when prompted. Switching to a T4 GPU runtime makes the AI tagging much faster.

> [!NOTE]
> The review dataset is not included in this repository. Collect your own export and record the date you collected it, since live review pages change.

To run locally instead:

```bash
pip install pandas matplotlib transformers torch vaderSentiment jupyter
jupyter notebook CX_Case_Study_Analysis.ipynb
```

| Output | Created by |
| --- | --- |
| `review_tags_for_checking.csv` | Section 3: every AI theme tag, for hand checking |
| `partner_cx_survey_qualtrics.txt` | Section 4: import via **Tools → Import/Export → Import survey** in Qualtrics |

## Sources

- [Aussie Broadband Group Annual Report 2026](https://wcsecure.weblink.com.au/pdf/ABB/03124593.pdf), pp. 6, 22, 30, 32 (FY26 = year to 30 June 2026)
- Telecommunications Industry Ombudsman, *Complaints Data Insights*, calendar quarters 2024–2026 (no figure found for Q2 2024 or Q2 2025)
- ProductReview.com.au and Trustpilot public reviews
- WhistleOut Awards 2026, Finder Awards 2026, CHOICE 2026

## Author

**Ashlyn (Lynn) Le**, Bachelor of Business Analytics, Deakin University

# Evidence Strategy: how we get review and comparison evidence legally

Status: proposal addendum, 2026-09-29. Complements sections 10, 18, and 21 of `03-software-proposal.md`.

## The finding
RedDB.ai does not disclose its data source. Every commercial peer that did rely on Reddit either failed to
obtain a license (GummySearch, closed), was cut off by Reddit (GigaBrain), or built on dumps that Reddit has
since had removed. Reddit is closing its public Data API to new requests and has no third-party route to its
own AI summaries. Copying RedDB's approach means building on a source that is contractually closed and in
active litigation. We will not do that.

## The reframe
The question is not "how do we get Reddit" but "how do we get owner experience evidence we are allowed to
own." Reddit was a proxy for three things: owner satisfaction, recurring complaints, and reliability. Each has
a legal channel, and two of them produce data nobody else has, which is also what Google's review guidance
and the March 2026 update reward.

## The evidence portfolio, per category

Tier A: proprietary evidence (our moat)
1. Owner panel survey. A five-minute survey to 150 to 300 verified owners per category through Pollfish,
   SurveyMonkey Audience, or Prolific. Questions: model owned, months owned, repairs, would-buy-again,
   satisfaction by criterion, top complaint in free text. Cost about $300 to $1,200 per category depending
   on sample. Refreshed yearly. This is the single most valuable data purchase available and the first thing
   worth spending money on after the pipeline works. Publishing it as original research is exactly the
   "quantitative measurements" and "evidence of your own" Google asks for.
2. First-party owner reviews on our site. Structured form (criterion ratings plus free text), optional
   receipt or serial upload for a "verified owner" badge, a sentiment-neutral incentive (sweepstakes entry)
   disclosed on every review, negatives never suppressed. Slow to start, compounds for years, and it is the
   only channel that scales with traffic at zero marginal cost.
3. Repair-partner data (later). Independent appliance repair companies hold first-year service data. Yale
   Appliance publishes theirs; others may share anonymized brand and model service counts in exchange for
   attribution and links. One or two partners would give reliability evidence no aggregator has.

Tier B: public-domain and API evidence
4. ENERGY STAR and DOE datasets: specs, efficiency, UPCs (already in the plan).
5. CPSC SaferProducts incident reports and Recalls: safety incidents by brand and model; a safety panel on
   every product page.
6. YouTube Data API: review videos and their public comments, processed within the 30-day window into
   observations with pointers. For washers, popular review videos carry hundreds of owner comments.
7. Best Buy and Walmart APIs: rating averages and counts, prices, availability, with attribution. Walmart's
   review text, if the endpoint returns it, is displayed only in the affiliate context and not stored beyond
   the allowed window (terms to verify).

Tier C: cited third-party evidence
8. Yale Appliance service-rate reports and similar published repair data: cite the number, link the source.
9. Professional reviews: score, one-line attributed quote, link.
10. Editorial and blog reviews discovered by search and verified as actually discussing the product.

Tier D: sparse licensed AI research
11. OpenAI or Anthropic web-search calls used as a research assistant for the editorial writer: one or two
    queries per product, our own prose summary stored with outbound links, citations displayed. Never
    branded as "what Reddit says", never used as a bulk feed, never Gemini grounding (storage prohibited).

Tier E: Reddit, legally
12. Link-only: discovery search finds relevant threads; we store the URL and title as a pointer and show
    "Discussions elsewhere" links. Linking is not ingestion.
13. Operator-entered observations: you read threads and enter observations by hand in the admin with the URL
    as provenance. Legal, and feasible at 10 products per category.
14. Apply for Reddit Data API access under the Responsible Builder Policy describing the use honestly; if a
    grant arrives with acceptable terms, the adapter turns on. Expect a no.

## Schema impact
No new tables beyond one: `survey_responses` (survey, respondent hash, product, answers jsonb, collected_at,
panel_provider). Survey and first-party reviews both produce `observations` rows with `source` set to
`owner_survey` or `site_review`, so scoring treats them like any other evidence with a higher authority
weight. Add `verified_owner` and `incentive_disclosed` flags to site reviews for FTC compliance.

## Scoring impact
Subjective criteria draw on Tier A first, then B, then C. Evidence floors are stated per tier so a criterion
scored only from Tier C shows lower confidence than one backed by survey data. Reliability uses survey repair
rates plus cited service-rate data plus CPSC incidents, and shows all three.

## Cost posture
Everything in Tiers B, C, and E is free. Tier D costs cents. Tier A item 2 costs development time only.
Tier A item 1 is the first recommended paid data purchase, and it can start at about $300 for a 150-owner
washer survey once the pipeline is proven.

## Decision requested
Adopt this portfolio in place of Reddit ingestion for the MVP, and approve a small owner survey for washers
as the first paid data item when Phase 1 is complete.

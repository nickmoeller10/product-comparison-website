# Niche Selection and Source Attribution

Status: discussion document, 2026-09-29. Supersedes the niche recommendation in `05-source-plans-and-niche.md`
where they differ.

## 1. Attribution: can we hide which sites the community discussion came from?

Short answer: storing the source privately is fine; hiding it publicly makes the position worse, not better.

1. It breaks the AI providers' terms. OpenAI's web search docs require that inline citations be "clearly
   visible and clickable" whenever web-derived information is shown to end users. Anthropic's docs require
   citations when search-derived output is displayed. Hiding sources violates the only license we have.
2. It removes the legal defense. Paraphrasing and short quotation for commentary is defensible because it
   is attributed commentary. Unattributed "community says" text looks like appropriation, not commentary.
3. It creates an FTC problem. Under the Consumer Reviews rule, AI-synthesized text presented as consumer
   opinion with no verifiable origin reads like fabricated reviews. Sources are what make it a summary.
4. It hurts SEO. Google's review guidance and the AI-citation research both reward evidence and links.
5. Reddit's own terms require attribution and link-back for any permitted use, so attribution is a
   condition, not a courtesy.

Design that satisfies everything: the section is titled "Community discussion summary", the body is our
paraphrase, and beneath it a "Sources" row lists the thread links. The row can be compact or collapsed, but
it must be visible and clickable. Internally every observation keeps its source URL and source type.

Provider choice: Gemini is out (its terms forbid storing grounded results to build a database). OpenAI is
preferred for Reddit-scoped research because it holds a Reddit license. Claude works for the same pattern
on non-Reddit sources, with the same domain filters and citation rules; both cost about $10 per 1,000
searches plus tokens. Storage rule for all providers: paraphrased observations plus URL pointers, never
verbatim review text.

## 2. The most lucrative niche

Ranked by expected commission per sale, data availability, return risk, and incumbent strength (full table
in `research/affiliate-niches.md`):

| Niche | $/sale | Public spec data | Incumbents | Return risk |
|---|---|---|---|---|
| Portable power stations and home backup | $50 to 150 | Excellent | Generalist tech sites | Low |
| HVAC: heat pumps, mini-splits, water heaters | $40 to 200 | Best available (ENERGY STAR, AHRI) | Weak | Low |
| Major appliances | $30 to 90 | Excellent | Consumer Reports, Yale | Low-moderate |
| E-bikes | $30 to 100 | Good | Moderate | Low-moderate |
| Robot mowers and outdoor power | $30 to 150 | Good | Moderate | Low |
| Engagement rings | $125 to 1,000 | Excellent (4Cs feeds) | Strong | Moderate |
| Mattresses | $30 to 150 | None | Dominant, one owner | High |
| Finance (cards, loans) | $50 to 400 per approval | n/a | Dominant, hit hardest in 2026 | Reversals |

Recommendation: one brand covering home energy, climate, and appliances. Everything that plugs into or
powers a house: portable power stations and home batteries, heat pumps and mini-splits, water heaters, EV
chargers, air conditioners, dehumidifiers and purifiers, laundry, kitchen, and cleaning. It is coherent for
topical authority, it sits entirely on the ENERGY STAR and DOE public datasets plus manufacturer specs, and
it holds the two best-paying physical categories found. Washing machines remain the pipeline proof because
they use the same data spine. The first revenue category becomes portable power stations, not robot
vacuums: five to ten percent on $800 to $2,000 through EcoFlow, Jackery, Anker, and Bluetti direct
programs, specs that are fully comparable, and low returns.

Why not the others: mattresses have no spec standard, twenty-plus percent returns, and four of the top
sites under one owner. Finance pays best per approval and was hit hardest by the March 2026 update, with
heavy regulation. Rings pay the most per sale but are a different business with no review-evidence angle
and an entrenched incumbent. Technology pays the least and is owned by lab-testing incumbents.

Suggested launch order: washing machines (proof), portable power stations (revenue), heat pumps and
mini-splits (data moat, high AOV), refrigerators and dishwashers, then robot vacuums and espresso for the
Amazon test.

## Decisions requested
1. Adopt the home energy, climate, and appliances niche.
2. Portable power stations as the first revenue category after the washer proof.
3. Show sources under community summaries as described in section 1.

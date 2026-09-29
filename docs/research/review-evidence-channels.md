# Reddit-derived evidence: how peers source it, and legal alternatives (29 Sep 2026)

Not legal advice. Several official pages were blocked by the sandbox proxy; items marked [snippet] or
[unverified] rest on search snippets or secondary write-ups.

## How RedDB.ai and peers source Reddit data
- RedDB.ai: describes itself as "rankings and comparisons for 3,600+ products, built from 1,100,000+ real
  Reddit discussions" across 58 categories. No founder, launch post, data-source statement, license, or
  affiliate disclosure was found. At that scale under the 2023 to 2026 API regime, the plausible sources are
  historical Pushshift or Arctic Shift dumps or scraping. No evidence of a Reddit license. [unverified]
- GummySearch (commercial): could not reach a commercial license agreement with Reddit after two-plus years;
  closed to new signups Nov 30 2025; deletes data Dec 1 2026. gummysearch.com/final-chapter [snippet]
- GigaBrain (commercial, had a $79/mo API): Reddit discontinued its data access after Reddit Answers launched;
  search shut down; earlier coverage described it as scraping. [snippet]
- RedditRecs, RedRecs, SmartBuysForLife: Reddit-comment aggregators with no stated license.
- BIFL-Recs: open-source hobby harvester via OAuth; non-commercial.
- Dumps: Pushshift closed May 2023; Academic Torrents dumps taken down at Reddit's request Jul 30 2026;
  Arctic Shift still exists but Reddit's terms bar commercial use without a contract.
- Pattern: every commercial Reddit-derived product tool examined either shut down, was cut off, or is silent
  about its source. None documents a license.

## Reddit's stance for small commercial apps
- Free tier (100 QPM) eligibility is non-commercial. Reddit Help: "you cannot use any Reddit developer tools
  and services for commercial purposes without first getting permission"; commercial means "any use by a
  business or on behalf of a business or as part of a monetized product or service." Public Content Policy
  (May 2024): commercial use requires a contract. Low volume does not change eligibility. [snippet]
- Responsible Builder Policy (Nov 11 2025): self-service access closed; every new OAuth client needs a
  support ticket describing use case, data, subreddits, volume; Reddit may deny without reason or appeal.
- Aug 5 2026 r/redditdev: Reddit "will gradually start restricting all new requests" to the public Data API;
  apps must register by Sep 30 2026 and move to the Developer Platform (Devvit), which runs on Reddit's
  servers and is not designed to export content. Named Data API partners: OpenAI, Google, Sprinklr.
- Developer reports 2025-26: weeks of waiting, generic rejections; commercial apps routed to a partner path
  with reported minimums near $12k/month. [snippet]
- Reddit Answers and the Feb 2026 AI shopping carousel are first-party; no third-party API.

## Licensed LLM search tools as an indirect path
- OpenAI Responses API web search: OpenAI has a Reddit license (May 2024); the tool can cite reddit.com but
  measured Reddit share of API citations is under 1 percent (Aug 2026). Customer owns Output; inline citations
  must be visible and clickable when web results are displayed. No stated retention cap. [snippet]
- Gemini Grounding with Google Search: terms require displaying Search Suggestions, forbid modifying or
  interspersing grounded results, cap storage of grounded text at 90 days for display evaluation, and state
  that using grounding "to extract or collect" components for another purpose violates the terms. Not usable
  to build a stored database. [snippet]
- Anthropic web search: powered by Brave; citations must be shown for direct display; no Reddit license.
- Perplexity Sonar: Perplexity is a defendant in Reddit v. SerpApi et al. Avoid.
- Assessment: "summarize discussion of product X, store our own prose summary plus URLs, display citations"
  is defensible only as sparse, cited editorial research, never as a Reddit-derived database and never
  branded as "Reddit says". Prefer OpenAI or Anthropic over Gemini. Yield of Reddit citations is low.

## Other legal evidence channels
- CPSC SaferProducts.gov incident reports (OData API, free key; brand, model, narrative, manufacturer comment)
  and CPSC Recalls API (UPC, hazard, remedy, units, injuries). Federal works, public domain. Safety evidence.
- Yale Appliance annual reliability reports: 33,190 first-year service calls (2026); service rate by brand
  and type; e.g. LG front-load 2.7 percent, GE Profile top-load 3.4 percent, Speed Queen 4.6 percent.
  Freely published; cite with attribution and link; regional and limited to brands Yale sells.
- Consumer Reports: no-commercial-use policy; brand licensing only; not usable.
- Threads keyword search API: exists but needs Meta App Review plus business verification; low appliance
  volume. Lemmy: open API, tiny volume. Mastodon.social bans scraping.
- Walmart Affiliate API reviews endpoint: exists; third-party wrappers show review text fields [unverified];
  terms allow use "solely for the purpose of advertising Walmart.com products" and ban syndication.
- First-party owner reviews: 16 CFR 465.4 bans incentives conditioned on sentiment; sentiment-neutral
  incentives (sweepstakes entry, small gift) are allowed if clearly disclosed on the review (16 CFR 255);
  465.6 bans company-controlled sites posing as independent; 465.7 bans suppressing negatives. Receipt or
  serial upload for "verified owner" is common practice.
- Panel surveys, 300 washer owners, about 5 minutes: Pollfish roughly $600 to 900; SurveyMonkey Audience
  $700 to 1,200; Prolific $500 to 800 plus a screener study. Output is proprietary, fully owned data.
- Complaint data: BBB is personal-use only; some state AGs publish complaint datasets (WA, MA, TX) with thin
  narratives; FTC Sentinel is law-enforcement only.

## Ranked channels
| Rank | Channel | Legal status | Cost | Evidence | MVP? |
|---|---|---|---|---|---|
| 1 | Owner panel survey | Clean; owned | $600 to 1,200 per 300 responses per category | Satisfaction, failure rates | Yes, first paid data item |
| 2 | First-party owner reviews with receipt verification and disclosed neutral incentive | Clean if 465/255 followed | Dev time plus prizes | Long-form owner experience | Yes, start early; compounds |
| 3 | Yale Appliance service rates, cited | Public; attribute and link | Free | Repair rates by brand | Yes |
| 4 | CPSC incidents and recalls | Public domain | Free | Safety | Yes |
| 5 | OpenAI or Anthropic web-search summaries with links | Output owned; sparse use only | Cents per query | Editorial cross-web context | Cautious |
| 6 | Walmart reviews endpoint | Affiliate purpose only; storage unverified | Free | Retail reviews | Maybe, display-only |
| 7 | State AG complaint data | Public records | Free | Complaint counts | Later |
| 8 | Threads, Lemmy | Gated or tiny | Free | Social mentions | No |
| 9 | Gemini grounding | Storage prohibited | Cheap | Summaries | No |
| 10 | Reddit commercial API | Contract, about $12k/mo, being restricted | High | Discussion | No |
| 11 | Reddit dumps, scraping, Perplexity Sonar | Contrary to ToS; litigation | Low | Discussion | No |

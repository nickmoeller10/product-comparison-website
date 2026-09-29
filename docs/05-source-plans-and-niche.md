# Source Plans, Evidence Mechanics, and the Niche Question

Status: discussion document, 2026-09-29. Extends `04-evidence-strategy.md`. Figures from
`research/` appendices; anything marked [verify] must be confirmed on the official page.

## 1. Survey panels: APIs and costs

| Panel | Programmatic launch | Cost per complete (5 questions plus one screener, US adults) | Notes |
|---|---|---|---|
| Prolific | Yes. REST API creates studies, sets eligibility, publishes, approves, exports. | About $0.60 to $0.90 plus small screen-out payments (recommended $12/hour pay plus 42.8 percent business fee) | No fixed "owns product X" prescreener; use custom screening with branching, screen-outs paid a reduced amount. Callable from n8n with the HTTP node. |
| SurveyMonkey Audience | No. The API creates surveys and fetches responses, but panel purchase is UI-only. | About $1 to $3 | API needs a higher paid plan [verify]. n8n has a trigger node for responses. |
| Pollfish | No researcher API. | From $0.95 plus targeting; about $2 or more with a screener | Dashboard only. |
| Cint Exchange | API exists but onboarding is through a sales team. | Roughly $1 to $3 | Not practical for a tiny buyer. |

Recommendation: Prolific through its API, driven by an n8n workflow that creates the study from the
category rubric, screens for ownership, and imports submissions into `survey_responses`. A 150-owner washer
survey lands near $150 to $250 all-in, lower than the earlier estimate.

## 2. First-party reviews: the "one month of Premium for a photo review" incentive

Legal as designed if four rules hold:
1. The incentive is never conditioned on sentiment. State it on the form: "You receive one month of Premium
   for any verified review, positive or negative."
2. Every incentivized review carries a visible disclosure: "Reviewer received one month of Premium for
   submitting this review."
3. Every review is published regardless of rating; moderation only for abuse, spam, or off-topic.
4. Verified owner: photo of the product plus a receipt, serial, or model plate. The photo doubles as
   evidence and as a fraud filter.
Before Premium exists, the same mechanism can pay with a sweepstakes entry or an "early supporter" badge.
Bonus: reviews collected this way are ratings "sourced directly from users", so the site may legitimately
show AggregateRating structured data, which third-party ratings never allow.

## 3. CPSC, YouTube, retailer APIs

Adopted: CPSC SaferProducts incidents and Recalls (public domain), YouTube Data API under the 30-day rule,
Best Buy Products API, Walmart Affiliate API (via Impact), Home Depot and Lowe's product feeds via Impact.
Target has no public API; it is linked through Impact only.

## 4. OpenAI web search scoped to Reddit

Mechanics (verified against the docs): the Responses API web search tool accepts
`filters.allowed_domains` (up to 100 domains, subdomains included), so a call can be restricted to
reddit.com. `tool_choice` can force the search. `search_context_size: "high"` pulls more content.
Cost: about $10 per 1,000 calls plus content tokens at the model's input rate, so roughly $0.03 to $0.06
per product query. Citations come back as URL annotations; the docs require inline citations to be visible
and clickable when web-derived information is displayed.

Prompt design (versioned in `/prompts/reddit_scoped_research.md`):
- Input: canonical product name, model numbers, known aliases, category, the rubric criteria list.
- Instruction: search only reddit.com; find threads where the exact model or a clear variant is discussed by
  owners; ignore threads that only mention the brand; return JSON.
- Output schema: `product_confirmed`, `threads[] {url, title, subreddit, approximate_date, relevance}`,
  `observations[] {criterion, polarity, claim_paraphrased, thread_url, confidence}`, `consensus_summary`,
  `disagreements[]`.
- Rules: paraphrase in our own words; no verbatim passages longer than ten words; do not invent; if fewer
  than two relevant threads exist return `product_confirmed: false` and nothing else; separate owner
  reports from speculation; note recency.
- Validation: schema check; URL must be a reddit.com thread; observations without a thread URL are dropped;
  a second cheap model spot-checks that the claim matches the criterion.

Legal posture, stated honestly: OpenAI holds a Reddit license and its terms give you ownership of the
output, but that license does not extend to you, and Reddit's user agreement restricts commercial use of
its content. A paraphrased summary with links is what any journalist produces, and at one to three calls
per product it is sparse editorial research, not a Reddit-derived database. Keep it sparse, store only our
paraphrases plus URLs, show the citations, label it "Community discussion summary" rather than "Reddit
says", and never batch-crawl subreddits through it.

## 5. Google's grounding terms in plain language

Google lets its Gemini model look things up on Google Search and answer with sources. The fine print says
four things. You must show Google's "search suggestion" chips next to the answer. You may not edit the
grounded text or mix other content into it. You may keep the grounded text for at most 90 days, and only
to check how it displays. And you may not use the feature to "extract or collect" the results for any other
purpose. Building a stored database from those answers is collecting for another purpose, so it is out.
OpenAI's terms, by contrast, give you the output and ask only for visible, clickable citations. That is
why the research recommends OpenAI or Anthropic for this and not Google.

## 6. Reddit links
Adopted: discovery search stores thread URLs and titles as pointers; product pages show a "Discussions
elsewhere" list. No ingestion.

## 7. Source plans for three products

### 7a. Washing machine (LG WM4000HWA as the worked example)
| Need | Sources, in authority order | Type |
|---|---|---|
| Identity and variants | ENERGY STAR dataset plus UPC dataset; DOE CCMS; manufacturer model page (color suffixes HWA, HBA) | Public domain, facts |
| Objective specs | ENERGY STAR (capacity 4.5 cu ft, IMEF, IWF, kWh per year, water per year); DOE CCMS; manufacturer page (dimensions, weight, spin RPM, cycle count, steam, Wi-Fi, warranty, MSRP) | Public domain, facts |
| Price and availability | Best Buy API, Walmart API, Home Depot and Lowe's feeds via Impact, LG direct via CJ; history from our own `price_points` | API, feeds |
| Reliability | Owner survey repair rate; Yale Appliance first-year service rate for LG front-load (cited, 2.7 percent in 2026); CPSC incidents and recalls; site reviews | Proprietary, cited, public domain |
| Cleaning performance | Editorial and lab reviews cited by score and link (Reviewed, Wirecutter verdict, Tom's Guide); YouTube test videos; owner survey satisfaction on cleaning | Cited, API, proprietary |
| Noise and vibration | Manufacturer dB rating where published; owner survey; YouTube comments; scoped community research | Facts, proprietary, API, sparse |
| Usability, app, cycle time | Owner survey; YouTube comments; site reviews; editorial mentions; scoped community research | Mixed |
| Recurring complaints | Observations aggregated by criterion across all evidence sources with counts | Derived |
| Value | Computed: overall score per dollar at current best price | Derived |

Rubric v1.0 sketch: cleaning performance 20 (subjective), reliability 20 (mixed), capacity 10 (objective),
efficiency 10 (objective from IMEF and IWF), noise 10 (objective if spec, else subjective), features and
usability 10 (subjective), owner satisfaction 10 (survey), value 10 (objective). Evidence floors: subjective
criteria need at least 8 observations from at least 2 source types, or they show as unscored.

### 7b. Water bottle (Hydro Flask 32 oz Wide Mouth as the example)
| Need | Sources | Notes |
|---|---|---|
| Identity | Manufacturer site; Walmart API; UPC from retailer listings | No federal dataset |
| Specs | Manufacturer (capacity, material, insulation claim, weight, lid type, dishwasher safe, BPA-free) | Facts |
| Performance | YouTube temperature-retention tests are common and useful; editorial reviews (OutdoorGearLab, Wirecutter) cited | Cited, API |
| Safety | CPSC recalls (lead-solder controversies, lid hazards) | Public domain |
| Owner experience | Site reviews; a cheap owner survey (ownership incidence is very high, so screening is nearly free); YouTube comments; scoped community research | Proprietary, API |
| Price | Walmart API; Amazon later; manufacturer | API |
Verdict: the pipeline works, but the economics do not. A $35 bottle at about 3 percent yields about $1 per
sale, the market is saturated with editorial coverage, and there is no public data moat. Water bottles are
a traffic category at best, not a revenue category.

### 7c. Shower head (a Kohler or Moen 2.5 GPM handheld as the example)
| Need | Sources | Notes |
|---|---|---|
| Identity and flow rate | DOE CCMS showerheads (certified flow rate); EPA WaterSense product database (public); manufacturer | Public data exists here |
| Specs | Manufacturer (spray settings, finish, hose length, mount type, pressure-compensating) | Facts |
| Performance | Objective: GPM and WaterSense certification; subjective: pressure feel from owner survey, YouTube, site reviews, editorial tests | Mixed |
| Install and durability | Owner survey; site reviews; scoped community research; CPSC | Mixed |
| Price | Home Depot and Lowe's feeds; Walmart API; Amazon later | API |
Verdict: about $1.50 per sale at Home Improvement rates, but high search volume, low editorial competition,
and a real public dataset. A good long-tail category inside a home niche, not a headline category.

## 8. The niche question

Yes, a niche helps SEO, and the research says so directly: Google's 2025 and 2026 updates judged sites at
the domain level, rewarded owned data and first-party expertise, and punished broad aggregation. One domain
that is "the data site for X" accumulates topical authority, a credible author, tight internal linking,
and depth per category. A thousand categories under one brand is the pattern that got sites removed.

Technology is the wrong niche for this business, for three reasons:
1. Commissions are the lowest in the market. Amazon pays 2 percent on TVs, 1 percent on consoles, 2.5
   percent on PCs, 3 percent on headphones; Best Buy pays about 0.5 percent on new customers and nothing on
   returning; Walmart electronics is reported at 1 percent. A $700 TV yields about $14 at Amazon and $3.50
   at Best Buy.
2. The incumbents own it with lab data. RTINGS runs physical test labs for TVs, monitors, headphones and
   now paywalls the results; Wirecutter, CNET, Tom's Guide and The Verge test hands-on. Google's review
   guidance rewards exactly that first-hand testing. A data-aggregation site cannot out-evidence a lab.
3. Product churn is annual, so the evidence corpus depreciates fast.

Home appliances and home systems is the right niche, for the mirror-image reasons:
1. Commissions can be high through brand and specialty programs. LG reportedly pays up to 12 percent on
   appliances direct, so a $900 washer yields about $108 and a $1,800 refrigerator about $216. Specialty
   appliance retailers pay 3 to 6 percent. Even Home Depot at 1 percent on a $1,500 basket beats a TV.
2. Public data is a moat. ENERGY STAR and DOE publish certified datasets for roughly 40 residential
   product types: washers, dryers, dishwashers, refrigerators, freezers, ranges, microwaves, room and
   central air conditioners, heat pumps, furnaces, water heaters, dehumidifiers, air purifiers, ceiling
   fans, thermostats, pool pumps, showerheads, faucets, toilets, EV chargers. CPSC covers incidents and
   recalls. Yale publishes reliability rates. No technology site has an equivalent.
3. Competition is thinner. RTINGS does not test appliances; Consumer Reports does but is paywalled and
   licenses nothing; most appliance content online is retailer copy.
4. Purchase intent is high and researched: people search for weeks before a $1,000 appliance.
5. Amazon-heavy subcategories live inside the niche for the Amazon test: robot vacuums (Home, about 3
   percent), espresso machines and coffee makers (Kitchen, 4.5 percent), air purifiers, dehumidifiers.

Estimated commission per sale (rates unverified; AOV assumed):
| Category | AOV | Best available program | Per sale |
|---|---|---|---|
| Washing machine | $900 | LG direct 12 percent; Lowe's 2 percent; Home Depot 1 percent | $108 / $18 / $9 |
| Refrigerator | $1,800 | LG direct 12 percent; specialty retailer 4 percent | $216 / $72 |
| Dishwasher | $800 | LG direct 12 percent; Home Depot 1 percent | $96 / $8 |
| Robot vacuum | $500 | Amazon Home 3 percent; Walmart up to 4 percent | $15 / $20 |
| Espresso machine | $500 | Amazon Kitchen 4.5 percent | $22.50 |
| Television | $700 | Amazon 2 percent; Best Buy 0.5 percent | $14 / $3.50 |
| Headphones | $150 | Amazon 3 percent | $4.50 |
| Laptop | $900 | Amazon 2.5 percent | $22.50 |
| Shower head | $50 | Home Improvement 3 percent | $1.50 |
| Water bottle | $35 | about 3 percent | $1 |

Recommendation: brand and domain the site as the home appliance and home systems data authority. Launch
order: washing machines (pipeline proof, LG program), robot vacuums (Amazon test), dishwashers and
refrigerators (revenue), then climate and water categories. Keep water bottles out. Keep technology out
until the appliance niche is established and the brand can carry a second family.

## Decisions requested
1. Adopt home appliances and home systems as the niche and drop the broad multi-category brand for now.
2. Prolific as the survey panel, integrated by API.
3. The review incentive as specified in section 2.
4. The OpenAI Reddit-scoped research workflow, sparse, with the prompt rules in section 4.

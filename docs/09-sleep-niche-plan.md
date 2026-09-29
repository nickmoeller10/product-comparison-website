# Sleep, Mattress, and Bedding: Niche Plan

Status: discussion document, 2026-09-29. Program rates and volumes marked [verify] are from the batch
research; the sub-category research appendix (`research/sleep-subcategories.md`) holds the sources.

## 1. Product taxonomy (what we can rank)

Every node below is a ranking page, a set of comparison pages, and a set of model pages. Modifiers
multiply pages, but only pages that pass the completeness gate are indexed.

### 1.1 Mattresses (core)
- By construction: memory foam, hybrid, innerspring, latex, airbed, organic and natural.
- By firmness: soft, medium, medium-firm, firm, extra-firm (normalized against independent tester ratings).
- By sleeper: side, back, stomach, combination; couples; heavy (over 230 lb); lightweight; seniors; kids; teens.
- By need: back pain, hip pain, shoulder pain, hot sleepers, motion isolation, edge support, allergies,
  adjustable-base compatible, fiberglass-free, low off-gassing, made in USA.
- By size and form: twin, twin XL, full, queen, king, California king, split king, RV and short queen,
  bunk-bed thickness, 8 inch and under, sofa-bed replacement, floor and Japanese-style.
- By price: under $500, under $1,000, under $2,000, luxury.
- By channel: bed-in-a-box, showroom brands, retailer (Mattress Firm, Costco, Amazon top sellers).
- Comparisons: brand versus brand and model versus model, the highest-intent queries in the niche.
- Brand hubs: one page per brand with lineup, warranty terms, price history, complaint profile.

### 1.2 Mattress accessories
- Toppers: memory foam, latex, feather, cooling, firming, by thickness.
- Protectors and encasements: waterproof, cooling, bed-bug, organic.
- Pillows: by sleeper position, cooling, memory foam, latex, down and down-alternative, adjustable,
  cervical and neck pain, body and pregnancy, travel, kids.
- Sheets: cotton percale, sateen, linen, bamboo and lyocell, microfiber, flannel, cooling, deep pocket,
  organic; ranked by GSM or thread count, weave, certifications, shrinkage and pilling from owner data.
- Comforters, duvets, inserts: down (fill power, fill weight), down alternative, all-season, cooling,
  organic; duvet covers.
- Blankets: weighted (by weight and fill), cooling, electric and heated, throws.
- Foundations and frames: box springs, platform beds, adjustable bases, bed frames by style, headboards,
  storage beds, floor and low-profile, bunkie boards.

### 1.3 Special-purpose sleep
- Kids: toddler beds, twin beds for kids, bunk beds, loft beds, trundle beds, kids' mattresses.
- Nursery: cribs, crib mattresses (the one sub-category with a federal firmness standard), bassinets,
  toddler pillows, mattress pads for cribs.
- Guest and flexible: sofa beds, sleeper sofas, futons, murphy beds, daybeds, air mattresses, folding beds,
  cots.
- Travel and outdoor: camping mattresses and pads, RV and camper mattresses, truck mattresses, travel
  pillows.
- Dorm and small space: twin XL packages, loft beds, mattress-in-a-box under $300.
- Pets: dog beds, orthopedic dog beds, cooling pet beds, crate mats.

### 1.4 Sleep tech and environment
- Smart beds and cooling systems: Eight Sleep, Sleep Number, BedJet, Chilipad and Sleepme, cooling toppers.
- Trackers and wearables: Oura, Withings, Whoop, Apple Watch sleep, under-mattress sensors.
- Environment: white noise machines, sunrise alarms, blackout curtains, air purifiers for bedrooms,
  humidifiers, bedroom fans, smart lights.
- Sleep aids that are products, not medicine: sleep masks, earplugs, anti-snore pillows, mouth tape
  (careful with health claims), sleep headphones.

### 1.5 Informational hubs that feed the database pages
- Mattress size chart and dimensions; bed frame sizing; sheet sizing and pocket depth.
- How long a mattress lasts; when to replace; how to fix sagging; how to dispose of or recycle a mattress.
- Fiberglass in mattresses: which brands use it, which states restrict it.
- Warranty guide: sag thresholds explained, how to file a claim.
- Sales calendars: Presidents Day, Memorial Day, July 4, Labor Day, Black Friday, with our price history
  showing what actually drops.

## 2. The review-for-access mechanic

Design: anyone can read aggregate ratings, the score breakdown, the editorial summary, and a sample of
reviews. To unlock the full review database (every review, filters by body type and sleeper position, the
durability-by-year charts, the complaint explorer), a visitor either submits one verified review of a
sleep product they own or subscribes. Each review requires: product and model, purchase month and year,
body weight band, sleeper position, criterion ratings, free text, and a photo of the product or law label.
Reviews are published regardless of rating after abuse moderation.

Why it works: it is a self-perpetuating data engine that costs nothing per review, every review is a
structured observation with verified ownership, and the reviewer population skews toward people mid-decision,
which is exactly who converts on affiliate links.

Legal rules that must hold:
1. The benefit is not conditioned on sentiment. State it on the form and in the terms.
2. Disclose the material connection on each review: "Reviewer received site access for submitting this
   review." The FTC Endorsement Guides treat any benefit as a material connection.
3. Never suppress negatives; moderate only for abuse, spam, off-topic, or unverifiable ownership.
4. Do not present the site as independent of its own reviews in a misleading way; the site collects and
   publishes them and says so.
5. Verification is real: photo plus purchase evidence; duplicate detection; rate limits.
6. Quality gate: a minimum word count and required structured fields, or the review is saved but does not
   unlock access. This prevents junk reviews written only to unlock.
7. Keep enough review content public that Google indexes it. Gated text is invisible to search, so the
   public layer must carry the aggregate numbers, the summary, and the best examples.

Structured data benefit: reviews collected this way are user-sourced, so AggregateRating markup is
legitimate on product pages. Third-party ratings never allow that.

## 3. The quiz

"Find your mattress" is the personalized-ranking engine with a friendly front end, so it costs no AI.
Inputs: sleeper position, body weight band, partner and their weight band, pain areas, temperature
preference, firmness preference, motion sensitivity, edge use, allergies or material preferences,
adjustable base, size, budget, delivery preference, trial importance. Each answer sets a weight or a
filter on the rubric criteria. Output: three ranked picks with the reasons in plain language, a "what to
avoid" line, and affiliate links; a "save your results" email capture that enrolls the visitor in price
alerts for those picks. Variants: find your pillow, find your sheets, find your topper, kid's bed finder,
crib mattress checker. Rules: no medical claims; results are suggestions; the methodology page explains
the scoring. The quiz is also a data source: aggregated quiz answers show what buyers want by segment.

## 4. Revenue channels, ranked by fit

1. Affiliate commissions: flat bounties and percentages from brand programs, with brand-specific promo
   codes (a large share of mattress affiliate revenue flows through codes). Multiple sellers per model.
2. Seasonal deal pages and price alerts: mattress buying is sale-driven; a price-history tracker that
   tells subscribers when a real drop happens is both a traffic magnet and a conversion engine.
3. Premium: personalized rankings saved, full review database, durability-by-year data, price alerts,
   weekly deal digest. One-week trial with card. One-time buying report per model.
4. Display advertising once sessions justify it (Mediavine or Raptive style networks have traffic
   minimums); low priority because it degrades the page.
5. Sponsored placements: only clearly labeled, never in rankings. Rankings are the trust asset.
6. B2B: complaint and return-reason analytics for brands; anonymized survey data licensing.
7. Adjacent affiliate: sleep trackers, cooling systems, bedroom environment; higher margins on tech.

Clawback management: commissions on mattresses are held through trials. Cash-flow planning must assume a
90 to 120 day lag and a return rate above 20 percent on tracked sales.

## 5. Traffic channels

1. Search: the taxonomy above, gated by completeness, expanded in batches; comparison pages and brand
   hubs first because they carry the highest intent and the least competition per query.
2. Owned data as citation bait: price history, warranty tables, fiberglass database, survey durability
   charts. These get cited by AI answer engines and linked by journalists.
3. Email: quiz results, price alerts, seasonal sale digests. Klaviyo is already connected for marketing
   sends; transactional through Resend.
4. YouTube: short data-driven videos (price history, warranty comparison) are cheap to produce from the
   database and earn affiliate clicks; also a legal evidence source.
5. Community: Reddit link presence by answering with data (no scraping), Pinterest for bedding visuals.
6. Partnerships: sleep clinics, chiropractors, and interior designers as referral partners for the quiz.

## 6. Legal data channels for this niche

| Source | What it gives | Basis |
|---|---|---|
| Manufacturer product pages, spec diagrams, warranty PDFs, law labels | Construction, materials, dimensions, weight, firmness claims, warranty sag thresholds, return terms | Facts; robots-respecting fetch |
| Our price tracker on brand and retailer pages | Daily price by model and size, discount depth, sale cadence | Facts we observe |
| Owner panel surveys (Prolific) | Durability by year, satisfaction by segment, returns and warranty outcomes | Owned |
| Site reviews with verification | Structured owner experience with photos | Owned |
| Quiz answers (aggregated) | Demand by segment | Owned |
| Certification directories: CertiPUR-US, OEKO-TEX, GOTS, GOLS, GREENGUARD Gold, eco-INSTITUT, Responsible Down Standard, Supima, Cotton Egypt Association | Material and safety certification per product or brand | Public directories |
| CPSC: SaferProducts incident reports, recalls, 16 CFR 1632 and 1633 flammability, 16 CFR 1241 crib mattress standard, ASTM F1427 bunk beds | Safety incidents and compliance standards | Public domain |
| State fiberglass laws and disclosure rules | Fiberglass database | Public law |
| FTC actions and consent orders | Brand trust signals | Public record |
| YouTube Data API | Review videos and comments (30-day rule) | API terms |
| Retailer APIs (Walmart; Wayfair feed via CJ) | Prices, availability, rating aggregates | Affiliate terms |
| Professional review sites | Score plus link | Fair-use citation |
| OpenAI or Claude web search, sparse | Editorial context with visible citations | Provider terms |
| Reddit | Thread links only | Linking |

## 7. Rankings the database can produce that incumbents do not

- Warranty strength ranking (sag threshold, non-prorated years, exclusions).
- Discount honesty ranking (days on sale, real drop versus list).
- Durability by year from owner data (sag rate at years one, two, three).
- Fiberglass-free ranking with disclosure quality.
- Density-disclosure ranking (who publishes foam density and what it is).
- Return experience ranking (fee, pickup, refund time, from owner reports).
- Total cost of ownership: price, expected life from durability data, warranty value.
- Size-fit tools: sheet pocket depth versus mattress height; frame compatibility.

## 8. Launch order within the niche

1. Mattresses: top 40 models across the major online brands; brand hubs; 20 comparison pages; sleeper
   and need modifiers only where evidence supports them.
2. Toppers and pillows: cheap to research, fast to rank, strong Amazon and brand programs.
3. Sheets and comforters: certification-driven, high volume, lower ticket.
4. Adjustable bases and frames: high ticket, sold alongside mattresses.
5. Kids and nursery: crib mattresses first (federal standard gives objective data).
6. Sleep tech: highest margins, smallest catalog.
7. Guest, travel, pets: long tail.

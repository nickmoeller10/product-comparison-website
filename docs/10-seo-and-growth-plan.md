# Automated SEO and Growth Plan (Sleep Niche)

Status: discussion document, 2026-09-29. Built on the Google policy research (`research/google-seo-policy.md`),
the March 2026 update findings, and the sleep niche plan (`09-sleep-niche-plan.md`).

## 0. The constraint that shapes everything

Google's 2025 and 2026 updates removed sites that looked like this one: many templated affiliate pages with
AI prose and no owned data. Sites with proprietary data and first-hand evidence gained. AI Overviews halve
click-through on top positions, and cited pages earn two to five times the clicks of uncited ones. So the
plan is not "publish more pages faster." It is "publish only pages that carry data nobody else has, expand
in measured batches, and make every page easy to cite." Automation does the data work and the plumbing;
the index stays small until Search Console proves each batch earns its place.

## 1. Page-type playbook, in order of attack

Ordered by intent per query and by how weak the incumbents are on that page type.

1. Brand-versus-brand and model-versus-model comparisons ("purple vs casper", "nectar vs dreamcloud").
   Highest purchase intent, thousands of pairs, incumbents cover only the top pairs, and our comparison
   engine renders each pair from the database with spec deltas, warranty deltas, price history overlays,
   and owner data. Generated on demand, indexed only for pairs with search demand and complete data.
2. Brand hubs ("saatva mattress review", "nectar mattress"). Brand terms run in the hundreds of thousands.
   One hub per brand: lineup table, warranty terms parsed, price history for each model, complaint profile,
   certifications, FTC history, fiberglass status. This is the page journalists and AI engines cite.
3. Model pages. Full data card, score breakdown with evidence, price history, owner reviews, alternatives.
4. Modifier rankings ("best mattress for side sleepers", "best cooling sheets"). Only where the rubric has a
   real basis for the modifier (survey data by sleeper position, cited lab heat results). Each is a filtered
   view of the same scores with a written rationale, never a re-shuffled listicle.
5. Data pages that exist nowhere else: warranty strength ranking, discount honesty by brand, fiberglass
   database, durability by year, density disclosure table, return experience ranking, sale calendar with
   real price drops. These are the linkable assets.
6. Tools: quiz, sheet-fit and pocket-depth calculator, mattress size chart with frame compatibility,
   "is this a real sale" checker by model, bunk-bed mattress thickness checker, crib mattress fit checker.
   Tools earn links and repeat visits.
7. Head terms ("best mattress") last. They are owned by Sleep Foundation, Wirecutter, and Consumer Reports.
   We compete there only after the domain has authority from the pages above.

## 2. The automated content pipeline

Everything below is an n8n workflow writing to Supabase, with the site rendering from the database. No
page is hand-written; every page is regenerated when its underlying data changes.

1. Catalog ingestion: manufacturer pages and warranty PDFs fetched on a schedule, specs and warranty terms
   extracted into structured fields, changes logged.
2. Price tracker: daily fetch of list and sale price per model and size from brand sites and retailer
   APIs; computes days-on-sale, real drop percentage, and sale-event history.
3. Evidence collection: YouTube reviews and comments, cited lab scores, retailer aggregates, sparse
   scoped web research with citations, CPSC incidents and recalls, certification lookups.
4. Owner data: Prolific surveys quarterly per sub-category; site reviews continuously through the
   review-for-access mechanic; quiz answers aggregated weekly.
5. Scoring and ranking: deterministic recompute on any change; personalized weights served live.
6. Page generation: editorial blocks written by the model from stored observations only, with citations to
   observation IDs; QA model checks claims against the data; drafts to the admin queue.
7. Completeness gate: a page is indexable only when specs coverage, evidence floor, source count, and
   unique-content ratio pass thresholds. Everything else is served with noindex.
8. Publishing: approve in bulk, ISR revalidation, sitemap regeneration, internal-link recompute.
9. Refresh: weekly incremental checks; page regenerated only when scores, prices, or evidence changed;
   "last verified" date shown on page.

## 3. On-page and technical automation

- Structured data generated from the database on every page: Product snippet with Review (named author,
  reviewRating), positiveNotes and negativeNotes, AggregateRating from our own user reviews only,
  BreadcrumbList, Organization, Person for the author, ItemList on rankings (harmless in the US).
- Internal linking computed, not hand-placed: every model page links to its brand hub, its category
  ranking, its top three comparisons by demand, and its alternatives; every comparison links both models
  and the ranking; every data page links the models it mentions. Recomputed on publish.
- Titles and descriptions templated from data with a per-page unique element (the headline number: sag
  threshold, real discount, survey satisfaction).
- Freshness signals that are true: last-verified date, price-as-of timestamp, evidence count with date of
  newest observation.
- Methodology page and author page linked from every ranking and comparison; AI-assistance disclosure on
  generated sections; affiliate disclosure adjacent to links; rel="sponsored" on all affiliate links.
- Performance: static rendering with ISR, images optimized, no client-side data fetching for content.
- Crawl control: sitemaps split by page type, noindex pages excluded, canonical on every variant, and
  parameterized filter views never indexed.

## 4. Index expansion protocol

- Batch 1: 40 mattress model pages, 12 brand hubs, 30 comparisons, 5 data pages, 2 tools, methodology,
  author. About 90 indexable URLs.
- Measurement: Search Console API pulled daily into Supabase (impressions, clicks, position, indexation
  status by URL). Dashboard in admin.
- Rule: a batch expands only when at least 70 percent of its URLs are indexed and the batch shows rising
  impressions over four weeks. Pages with zero impressions after eight weeks are reviewed; if the data is
  thin they move to noindex automatically.
- Batch 2: toppers and pillows (models, comparisons, rankings). Batch 3: sheets and comforters. Batch 4:
  bases and frames, kids, cribs. Batch 5: sleep tech and long tail. Head terms only after batch 4.
- Cadence target: 50 to 150 new indexable URLs per month in year one, never thousands.

## 5. Earning links and citations without a PR team

Links are the input Google still weighs, and citations are what AI engines weigh. Both come from data.

1. Quarterly data studies generated from the database: "Which mattress brands actually discount, and by
   how much" (price history), "Sag rates by brand at year three" (survey), "The fiberglass map: which
   brands, which states" (CPSC plus law), "Warranty strength index". Each is a page with charts, a
   downloadable CSV, and a methodology. Journalists and brands link to numbers.
2. Journalist-request platforms (Qwoted, Featured, Help a B2B Writer, and Connectively's successors):
   an n8n workflow watches for sleep and mattress queries and drafts responses from the database with a
   citation offer; you approve and send. Ten minutes a day.
3. Brand-side amplification: brands that rank well are told, with a badge and a link; many repost. Never
   sell the badge.
4. Tool embeds: the sheet-fit calculator and size chart offered as embeddable widgets with attribution.
5. Data licensing to writers: free use of our charts with a link. Make it explicit on every data page.
6. Wikipedia-grade references: keep the methodology and data pages reference-quality so they are cited.

## 6. Traffic beyond Google

1. Email is the compounding channel. Capture points: quiz results, price alerts on any model, "notify me
   when this is a real sale", the weekly sleep deals digest, review submission. Sends: transactional through
   Resend, marketing through Klaviyo (already connected). Automated flows: price-drop alert, sale-event
   preview (the week before Presidents Day, Memorial Day, July 4, Labor Day, Black Friday), quiz follow-up
   with alternatives, review reminder at 90 days after purchase.
2. YouTube from the database: short videos generated from data (price history of a model over a year,
   warranty comparison of five brands, "is the Black Friday mattress sale real") with voiceover from a
   script the pipeline writes. Cheap, evergreen, and YouTube is itself a legal evidence source.
3. Pinterest for bedding visuals: sheets, comforters, bedroom setups, size charts; automated pin creation
   from page images with UTM tracking. Pinterest traffic converts on bedding.
4. Reddit and forums, by hand and legally: answer questions with data and a link, in the subreddits'
   rules. No scraping, no bots. The site's data pages are the thing people link to in threads.
5. AI answer engines: be the citable source. Every claim quantified, every page with a summary block that
   reads well when quoted, structured data complete, freshness visible. Cited pages get two to five times
   the clicks of uncited ones.
6. Google Discover: data studies and sale-event pages with strong images qualify; not a strategy, a bonus.
7. Bing and DuckDuckGo: submit sitemaps to Bing Webmaster Tools; IndexNow ping on publish. Free share.
8. Partnerships: sleep clinics, chiropractors, physical therapists, interior designers, and dorm-move
   guides link to the quiz. Offer a co-branded quiz result page.
9. Paid: none for Amazon-bound traffic (disqualified) and none until unit economics are known. Later,
   small tests on brand-versus-brand queries where a bounty covers the click cost.

## 7. Measurement and automation of the growth loop

- Search Console, site analytics, affiliate network reports, and email metrics pulled into Supabase daily.
- Admin growth dashboard: indexed URLs by batch, impressions and clicks by page type, top rising queries,
  pages losing impressions, affiliate clicks and earnings per page, email list growth, alert subscribers.
- Automated actions: noindex thin pages; propose new comparison pairs when a pair shows impressions
  without a page; propose modifier pages when queries with that modifier appear in Search Console; flag
  models with rising clicks but no price data; re-queue pages whose evidence is stale.
- Weekly review by you: approve proposed pages, approve journalist replies, look at losers.

## 8. Twelve-month targets (conservative, to be recalibrated with real Search Console data)

| Quarter | Indexed URLs | Organic sessions per month | Email list | Notes |
|---|---|---|---|---|
| Q1 | 90 to 150 | 2,000 to 8,000 | 500 | Comparisons and brand hubs indexing; first data study |
| Q2 | 250 to 400 | 10,000 to 30,000 | 3,000 | Toppers, pillows, sheets batches; second study; first journalist citations |
| Q3 | 500 to 700 | 30,000 to 80,000 | 10,000 | Bases, kids, cribs; sale-event pages before Labor Day |
| Q4 | 700 to 1,000 | 80,000 to 200,000 | 25,000 | Black Friday cycle; head-term attempts begin |

Affiliate revenue lags traffic by the trial window. Do not judge the niche on first-quarter earnings.

## 9. What not to do

No mass page generation, no thin modifier pages, no guest posts or link buying, no sold subfolders, no
aged domain, no scraped reviews, no AI text presented as consumer reviews, no rankings for sale, no
cloaked links, no paid traffic to Amazon.

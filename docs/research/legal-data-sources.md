# Legal source landscape for a washing-machine product-intelligence database (US, Sep 2026)

Not legal advice. Legend: (a) explicitly allowed; (b) allowed with license or paid tier; (c) prohibited by
terms; (d) legally grey. Items marked [verify] were read from snippets or secondary sources.

## 1. Reddit
- Governing docs: Data API Terms (revised July 2026 [verify]), Developer Terms, Responsible Builder Policy.
- Access model: every developer must request access and get explicit approval before pulling any data; manual
  approval with no SLA; multi-week waits and unexplained rejections reported.
- Free tier: 100 queries/min per OAuth client; non-commercial only (a).
- Commercial: terms prohibit deriving revenue from use of the Data API without express written approval. A
  monetized product-intelligence database (affiliate, subscriptions, B2B) is commercial (b).
- License cost: no self-serve tier; secondary sources report about $0.24 per 1,000 calls with an entry contract
  around $12k/month, and enterprise data licenses $50k to $500k+/yr.
- Retention: must delete content users delete or moderators remove; Reddit recommends purging stored content
  within 48 hours; retaining deleted content even anonymized is a violation; attribution and link-back required;
  no AI/ML training without a license.
- No authorized resellers. Reddit sued SerpApi, Oxylabs, AWMProxy and Perplexity (S.D.N.Y., Oct 2025) over
  scraping via Google results; on July 31 2026 the court largely denied dismissal. Buying scraped Reddit data or
  reaching threads via Google-scraped results is (c)/(d) and in active litigation.
- Discovering threads via search then fetching with the free API for a commercial product is still commercial
  use of the API (c) unless approved.

## 2. YouTube Data API v3
- Available: video metadata, channel data, search, public comment threads, statistics. Captions of others'
  videos are not available (owner OAuth only). No official transcript route for third-party videos.
- Quota: 10,000 units/day (search.list costs 100 units, so about 100 searches/day). Extensions need a
  compliance audit.
- Storage (Developer Policies III.E.4): public API data may be stored up to 30 calendar days, then deleted or
  refreshed; displayed data must be current; derived numeric metrics may be kept up to 36 months for approved
  clients; titles, descriptions, creator names and comment text stay on the 30-day rule; user deletion
  requests honored within 7 days; comments shown with channel identity and unmodified. (a) with 30-day refresh.
- Transcript scrapers (youtube-transcript-api, yt-dlp, actors): prohibited by ToS (c), legally (d).

## 3. Amazon
- PA-API 5.0 retired May 15 2026; replaced by the Creators API. Eligibility: approved Associate; reportedly 10
  qualified sales in the trailing 30 days, access revoked after a 30-day gap [verify]. Rate limit 1 req/s to start.
- Reviews: no third-party access to review text exists (c). Creators API may omit star fields [verify].
- Operating Agreement: no data mining or extraction tools; prices/availability cached at most 1 hour [verify],
  titles/descriptions 24 hours; images hot-linked only; prices shown must come from the API with an "as of"
  timestamp; affiliate links non-cacheable. (b) conditional on Associate status.

## 4. Retailer review data
| Retailer | Status | Reviews | Terms |
|---|---|---|---|
| Best Buy Developer API | Live | customerReviewAverage and customerReviewCount (aggregates only) | Attribute to Best Buy; no use for third-party retailers or price analysis; affiliate ID needed for commerce; cache windows [verify]. (a) aggregates with attribution |
| Walmart Affiliate API (walmart.io via Impact) | Live | reviewStatistics and review items | Requires Impact publisher account; ToS [verify]. (b) |
| Home Depot | No public API; product feed via Impact only | No | (c)/(d) |
| Lowe's Developer Hub | Partner-only | Partner-gated | (b) if accepted |
| Target | No public API | No | (c)/(d) |
- Bazaarvoice/PowerReviews (used by LG, Samsung, Whirlpool, GE sites): keys issued only to their clients;
  no aggregator access (c).

## 5. Search as discovery
- Google Custom Search JSON API: closed to new customers (Jan 2026); shut down Jan 1 2027; no caching or
  modifying results allowed. (c) for new users.
- Bing Web Search API retired Aug 2025.
- Brave Search API: free tier removed Feb 2026; $5 per 1,000 with $5 monthly credit; standard terms do not
  permit storing results; storage rights are enterprise. (b) for storage.
- SerpAPI (about $15 per 1,000), Serper.dev (about $1 per 1,000, results may be kept), DataForSEO ($0.60 to $2
  per 1,000): all scrape Google, which Google's ToS prohibits (d). SerpAPI is a defendant in the Reddit suit.

## 6. Scraping legal landscape (US)
- hiQ v. LinkedIn: scraping public pages is not CFAA unauthorized access, but hiQ still paid $500k and admitted
  breach of contract; CFAA-safe is not contract-safe.
- Meta v. Bright Data (2024): logged-off scraping of public data did not breach Meta's ToS.
- Ziff Davis v. OpenAI (Dec 2025): robots.txt is not a DMCA technological measure; still a good-faith signal.
- Reddit v. SerpApi (2025-26): DMCA and conspiracy claims survive for scraping via Google to evade blocks.
- Infrastructure: Cloudflare blocks AI bots by default; from Sep 15 2026 blocks mixed-use crawlers on ad-bearing
  pages by default; RSL licensing standard adopted by Reddit, Yahoo, Quora, Ziff Davis, Medium.
- Managed providers: Firecrawl respects robots.txt by default but puts target-site responsibility on the user;
  Apify requires lawful use but does not enforce robots.txt by default; none indemnify against a target's ToS.
  Public factual data, logged out, robots-respecting is (d) leaning defensible; behind login or click-wrap is (c).

## 7. Authoritative spec sources (fully clean)
- ENERGY STAR Certified Residential Clothes Washers: data.energystar.gov dataset bghd-e2wd (SODA API, CSV,
  OData). Fields: brand, model number, type, volume (cu ft), IMEF, IWF, annual kWh, annual water, date
  certified. Companion UPC dataset 8edu-y555. Federal work, public domain. Free app token, about 1,000
  requests per rolling hour. (a)
- DOE Compliance Certification Database (CCMS-4 Clothes Washers): every certified basic model including
  non-ENERGY STAR, with capacity, IMEF, IWF; downloadable; updated about every two weeks. (a)
- FTC EnergyGuide: label values come from DOE data; no separate API.
- Manufacturer spec pages and PDFs: facts (dimensions, capacity, cycles) are not copyrightable; store numbers
  and link the source; do not store full marketing text or images. (a) for facts.
- UPC lookup: UPCitemdb free 100/day, no redistribution; Barcode Lookup $99 to $249/mo; GS1 US Data Hub API
  $500 to $6,500/yr. (b)

## 8. Professional review sites
- RTINGS: no API; since March 2026 full results paywalled to deter AI reuse. Consumer Reports: No Commercial
  Use Policy; licensing required. Wirecutter: licensing via Wright's Media (four figures). CNET, Tom's Guide,
  Reviewed: no APIs; Ziff Davis is an active plaintiff.
- Fair-use position (not advice): a numeric score, title and link are facts; a short attributed quote for
  commentary is the classic fair-use pattern; bulk tables and paywalled data are not. (b) beyond score plus link.

## 9. Other sentiment sources
- Bluesky: ToS permits systematic retrieval through APIs provided for the purpose; firehose/Jetstream and public
  XRPC are those; must propagate deletions and honor user preferences. (a). Low washing-machine volume.
- Mastodon: public timelines without auth; per-instance ToS. (a)/(d)
- YouTube comments via Data API under the 30-day rule. (a)
- Trustpilot: business-level APIs only for paying businesses. (b), low value.
- ConsumerAffairs: personal non-commercial ToU. (c). Quora: scraping prohibited. (c). Forums: per-site ToS. (d).
- Manufacturer-site reviews: Bazaarvoice keys unavailable. (c)

## Recommended legal source stack, washing machines MVP
| Source | Access | Cost | May store | Must not store | Retention |
|---|---|---|---|---|---|
| ENERGY STAR washers + UPC datasets | SODA API / CSV | $0 | Everything | none | Indefinite; re-sync monthly |
| DOE CCMS clothes washers | Bulk download | $0 | All certified model specs | none | Indefinite; re-sync biweekly |
| Manufacturer spec pages/PDFs | Robots-respecting fetch, logged out | $0 | Extracted facts plus link | Full copyrighted text, images | Indefinite for facts |
| Best Buy Products API | API key plus Impact ID | $0 | SKU, model, timestamped price, rating aggregates, link, attribution | Review text (not provided), pre-effective prices | Refresh frequently; verify cache window |
| Walmart Affiliate API | walmart.io key via Impact | $0 | Item data, review aggregates, link | Redistribution beyond affiliate use | Per ToS [verify] |
| Amazon Creators API | Approved Associate with sales | $0 | ASIN, title, link, price with timestamp | Images, review text, stale prices | Price 1h, title 24h |
| YouTube Data API | API key | $0 | Video IDs, titles, channel, comment text and author, stats | Captions; unrefreshed data | Delete or refresh within 30 days |
| Reddit Data API | Apply; commercial contract | about $12k/mo entry (reported) | Attributed linked quotes under license | Deleted content; scraped data | Honor deletions; 48h purge |
| Bluesky firehose | Jetstream, no auth | $0 | Public posts, DID, permalink | Deleted posts; opted-out users | Propagate deletes |
| Professional reviews | Manual citation | $0 | Score, one-line attributed quote, link | Tables, paywalled data | Re-check periodically |
| Discovery search | Brave (no storage) or Serper/DataForSEO (grey) | $5 per 1k / $0.60 to $2 per 1k | Result URLs as pointers | Cached SERP content | Fetch and discard |

Excluded: Amazon, Home Depot, Target, Lowe's and manufacturer review text via scrapers or Bazaarvoice keys;
ConsumerAffairs; Quora; transcript scrapers; Google CSE; any scraped Reddit data.

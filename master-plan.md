# RateMyBed Master Plan

Version 1.1, 2026-09-29. Status: FOR OPERATOR REVIEW after two independent review passes (architecture and
feasibility; legal, SEO, trust, and revenue) whose 65 findings are incorporated. This document is the single
source of truth for scope, architecture, and build order. Where it conflicts with an earlier document in
`docs/`, this document wins. Research evidence lives in `docs/research/`. Conventions every build session
must follow live in `CLAUDE.md` and are kept consistent with this plan.

---

## Table of contents

1. [Executive summary](#1-executive-summary)
2. [Vision, principles, non-negotiables](#2-vision-principles-non-negotiables)
3. [Business model and revenue streams](#3-business-model-and-revenue-streams)
4. [Product specification](#4-product-specification)
5. [System architecture](#5-system-architecture)
6. [Technology stack and tools](#6-technology-stack-and-tools)
7. [Data model](#7-data-model)
8. [Backend design](#8-backend-design)
9. [Workflow catalog](#9-workflow-catalog)
10. [AI task registry](#10-ai-task-registry)
11. [Frontend design](#11-frontend-design)
12. [Entitlements and payments](#12-entitlements-and-payments)
13. [Data acquisition and legal posture](#13-data-acquisition-and-legal-posture)
14. [SEO and growth engine](#14-seo-and-growth-engine)
15. [Social autopilot](#15-social-autopilot)
16. [Operations and observability](#16-operations-and-observability)
17. [Security, privacy, and compliance](#17-security-privacy-and-compliance)
18. [Cost model and unit economics](#18-cost-model-and-unit-economics)
19. [Build plan](#19-build-plan)
20. [Scaling path](#20-scaling-path)
21. [Risks and mitigations](#21-risks-and-mitigations)
22. [KPIs and targets](#22-kpis-and-targets)
23. [Open decisions and assumptions](#23-open-decisions-and-assumptions)
- [Appendix A: document index](#appendix-a-document-index)
- [Appendix B: glossary](#appendix-b-glossary)
- [Appendix C: review log](#appendix-c-review-log)

---

## 1. Executive summary

RateMyBed (ratemybed.com) is a review platform for beds and sleep products. Verified owners rate their beds
in a structured form; those ratings are public and indexable. On top of the ratings sits an intelligence
layer: sentiment summaries derived from collected third-party reviews, objective spec and warranty data,
first-party price history, a versioned scoring rubric, comparisons, rankings, and a quiz that routes
visitors to the right products. Submitting one verified review unlocks the analytics layer; subscribing
adds saved rankings, reports, and the full durability dataset. Revenue comes first from affiliate
commissions on top-rated products and comparisons, then from premium access, and later from brand
analytics and data licensing.

The system is designed to be run by one person. All state lives in Postgres (Supabase) with provenance
on every fact. n8n orchestrates collection, extraction, scoring, page generation, moderation, alerts, and
growth loops as thin, idempotent workflows. AI is used only where interpretation is required (extraction,
classification, summarization, QA, moderation triage, photo checks) through a model registry with
reserved-and-settled budgets; everything mathematical is SQL. The site is a Next.js application on Vercel
that renders visitor-safe pages from published views and fetches gated content only for signed-in users.
The admin mission control lives in the same application.

Google's 2025 and 2026 updates removed sites built from templated affiliate pages and demoted platforms
that only aggregate other people's content. RateMyBed is built to be the other kind: native reviews,
native scoring, first-party price history, real comparisons, and a small index that grows only on Search
Console evidence. Costs are near zero during build; the first unavoidable costs (Vercel Pro when the
first batch is indexed, Supabase Pro when real user data exists) are about $50 to $70 per month plus
metered usage. Unit economics are modeled conservatively: at 13,000 monthly sessions the affiliate pool
is roughly $1,500 to $2,500 per month, which sets the timing for premium and brand revenue.

## 2. Vision, principles, non-negotiables

Vision: the place people go to find out what a bed is actually like to own, from people who own it and from
every credible source, organized so the right bed for a specific person is obvious.

Principles (engineering requirements):

1. Trust first. Every claim traces to a stored row with provenance. Every generated sentence has a citation
   row. Nothing is published that the data does not support.
2. Value in five things: native reviews, native scoring, comparisons, review summaries, suggestions.
3. One operator. Every component is operable through dashboards and approval queues. Weekly operator load
   target: under four hours after launch.
4. Zero spend until unavoidable. Free tiers during build; paid tiers when a limit or a commercial rule forces
   them; every metered call reserved against a budget before it runs.
5. Autonomy with oversight. Pipelines run themselves; publication, moderation edge cases, and growth
   proposals pass through queues the operator clears in bulk.
6. Data outlives tools. Postgres is the asset; site, orchestrator, AI provider, payment processor, and
   collectors are replaceable.
7. Small index, deep pages. Expand only on Search Console evidence, never on generation capacity.
8. Run as a review platform: verification, moderation, published policy, brand responses under moderation,
   enumerated dispute grounds, disclosed incentives, no suppression, DMCA agent, repeat-infringer policy.

Non-negotiables: no third-party review text republished; no collected reviews in the native section or in
AggregateRating; no Reddit scraping; no Gemini grounding for stored data; no Amazon Associates account while
Amazon collection runs; no bought or required links; no paid placement in rankings; affiliate disclosure
adjacent to every affiliate link with rel="sponsored"; no moderation action keyed on rating value.

## 3. Business model and revenue streams

Ordered by activation. Each stream has a trigger so nothing is built before it can earn. Figures are
conservative per the review (section 18).

| # | Stream | Mechanism | Trigger | Notes |
|---|---|---|---|---|
| 1 | Affiliate commissions (primary) | Brand programs (DreamCloud $150 flat, Purple to $150, Nolah $105 to $160, Leesa $75 paid after 125 days, Saatva 3%, Helix 6 to 12%, Parachute ~15%, Cozy Earth to 25%, Eight Sleep to $180), retailers (Wayfair 7%, Mattress Firm 3 to 4%), sub-affiliate networks (Sovrn, FlexOffers) applied for at skeleton launch, approval-gated | Skeleton site with policies live | Commissions reverse on returns inside 100 to 365 night trials; model 25 to 30 percent reversal and a 120 to 365 day lag; sub-affiliate networks keep about 25 percent; amazon.com links are excluded from any network auto-conversion while Amazon collection runs |
| 2 | Network-issued promo codes | Codes stored with authorization, validity window, and exclusivity flag; only network-issued codes displayed | With stream 1 | Leaked or unauthorized codes forfeit commissions; W18 validates |
| 3 | Premium membership | Monthly and annual; the premium-only assets are the full durability dataset by ownership year, saved personalized rankings, unlimited alerts, reports, and the weekly digest; seven-day trial with card; auto-renewal disclosure and one-click cancel per state auto-renewal laws | 10k organic sessions per month and 300 native reviews | $6 to $9 per month, $49 to $69 per year; budget 30 to 100 members in year one |
| 4 | One-time reports | Per-model or per-category report with price history and durability data | With premium | $4 to $12 |
| 5 | Brand accounts | Claimed pages, moderated responses, aggregated analytics for own products (minimum cell size 5); badges are free to any brand that earned a rank, with an optional nofollow link and no anchor requirement; payment buys analytics only | 50 or more native reviews for the brand's products | $49 to $199 per month; no effect on scoring or ranking |
| 6 | Brand analytics and data licensing | Aggregated complaint frequency, return reasons, segment satisfaction, competitor benchmarks; aggregated licensing covered by the user content license and privacy notice; minimum cell size 5; deletion cascades to exports by contract | 5,000 native reviews | Quarterly reports or API |
| 7 | Deal and sale-event sponsorship | Labeled sponsorship in the deals digest and sale pages, never in rankings | Email list of 10,000 | Flat fee per send |
| 8 | Adjacent lead generation | Mattress removal and recycling, sleep clinics, adjustable-bed installers | Traffic on those page types | Per-lead payouts; TCPA compliance for call leads |
| 9 | Display advertising | Ad network | 50k sessions per month and pages stay fast | Low priority |
| 10 | Public API and data feeds | Read-only keys over published views | Growth stage | Usage-based |
| 11 | Embeddable tools and white-label quiz | Sheet-fit calculator, size chart, quiz | After tools exist | Free with attribution; license for white-label |

Rankings and scores are never for sale. Anything a brand pays for is labeled and sits outside the scoring
path.

## 4. Product specification

### 4.1 Users and roles

| Role | Who | Can do |
|---|---|---|
| Visitor | Anyone | Read everything public; compare up to three products; quiz with basic results; price alerts on up to three quiz picks by confirmed email |
| Reviewer (unlocked) | Visitor whose review was published | Analytics layer for 12 months from publication, renewed by a published follow-up; saved items; alerts on five products |
| Member (Premium) | Paying subscriber | Reviewer capabilities plus the full durability dataset, saved personalized rankings, unlimited comparisons and alerts, full price history, weekly digest, reports within limits |
| Brand (claimed) | Verified brand representative | Moderated responses, aggregated own-product analytics, correction requests on data cards; no effect on scores |
| Operator (admin) | Nicholas Moeller | Mission control, moderation, publishing, rubric versions, budgets, rollbacks |

### 4.2 Site map and page types

All routes render from published views. Indexability is decided by one function, `index_gate` (section
8.2), and stored only in `pages.indexable`.

| Route pattern | Page type | Indexable when |
|---|---|---|
| `/` | Home: quiz entry, top rated, recent reviews, sale calendar | Always |
| `/rate/` and `/rate/[model]` | Review submission flow | Never |
| `/reviews/[brand]/[model]/` | Model page | `index_gate` passes |
| `/brands/[brand]/` | Brand hub: lineup, warranty terms, price history, complaint profile, certifications, public-record history, fiberglass status, correction channel | 3 or more indexable models |
| `/compare/[a]-vs-[b]/` | Comparison page | Both models indexable and pair has a demand signal |
| `/best/[category]/` and `/best/[category]/[modifier]/` | Rankings | 8 or more indexable models; modifier only where the rubric has evidence for it |
| `/quiz/[type]/` | Quiz landing | Landing indexable; results never |
| `/data/[study]/` | Data studies and tools | Once published |
| `/deals/` and `/deals/[event]/` | Price-history-backed sale pages, phrased as observed prices | Always; regenerated per event |
| `/methodology/`, `/review-policy/`, `/about/`, `/disclosure/`, `/privacy/`, `/terms/`, `/dmca/` | Trust pages | Always |
| `/account/*` | Saved items, alerts, membership | Never |
| `/admin/*` | Mission control | Never; role-gated |
| `/go/[click_id]` | Affiliate redirect | Never; robots.txt disallow plus X-Robots-Tag noindex; 302, referrer preserved, subid passthrough, merchant deep link untouched |

### 4.3 Review platform

Submission form (required unless noted):
- Product: brand, model, size, purchase month and year, price paid (optional), retailer, variant where the
  model has variants.
- Sleeper context: position(s), body weight band (under 130, 130 to 230, over 230), partner (and band),
  primary need.
- Ratings (1 to 5): comfort and support, durability so far, temperature, motion isolation, edge support,
  delivery and setup, customer service, value; overall. Stored as rows in `review_ratings` keyed to
  `rubric_criteria`.
- Text: likes (minimum 40 characters), dislikes (minimum 40 characters), comment (optional).
- Evidence: a photo of the bed with no people in it (required); receipt, order email, or serial (optional,
  raises verification tier). Law-label photos are accepted as evidence but never displayed.
- Attestations: 18 or older; truthful review; material-connection question ("Do you work for, sell for,
  or receive payment from this brand or a retailer?") with a visible disclosure on the review when yes;
  content license; incentive disclosure acknowledgement; marketing email opt-in as a separate checkbox.

Verification tiers: Tier 1 "Photo verified" (photo passed checks); Tier 2 "Verified purchase" (photo plus
purchase evidence). Both display; weights differ in scoring (8.2), never in AggregateRating (11.4).

Status machine: `submitted` to `moderating` and `verifying` (parallel) to `published`, `needs_human`,
`rejected`, or `removed`. A review publishes only through `publish_review()` under a row lock when both
moderation and verification results are clean. Between submission and publication the reviewer sees
"pending." Follow-up updates pass through the same moderation.

Moderation: AI pre-screen for abuse, spam, near-duplicates (trigram similarity), prohibited content
(personal data, threats, defamation of individuals), and material-connection inconsistencies. Categories are
enumerated in the Review Policy. No action keys on rating value. Human queue for flags; appeal by email;
every decision logged.

Follow-ups: at 6, 12, and 24 months, a short update (durability, sag observed, still recommend, comment).
A published follow-up extends the unlock 12 months from its own publication.

Brand responses: one plain-text response per review, no links, pre-publish moderation through the same
classifier and queue, labeled, logged. Brand correction requests on data cards go to a queue with a
required source.

Disputes: enumerated grounds only (reviewer is not an owner; prohibited content; provable falsity with
evidence). Reviews stay published during a dispute. Outcome counts are published quarterly on the Review
Policy page.

### 4.4 Access tiers

| Capability | Visitor | Reviewer (unlocked) | Premium |
|---|---|---|---|
| Reviews, scores, sentiment summary (total count and number of sources), rankings, comparisons up to 3 | Yes | Yes | Yes |
| Review filters by body band, position, need | No | Yes | Yes |
| Sentiment by source type, complaint frequency table | No | Yes | Yes |
| Durability by ownership year (full dataset) | No | Headline chart | Full |
| Price history | Last 30 days | 12 months | Full plus unlimited alerts |
| Personalized weighted rankings | Preview | Full, not saved | Full, saved |
| Quiz | Basic results | Full results with alternatives | Full plus saved profiles |
| Alerts | 3 quiz picks by confirmed email | 5 products | Unlimited plus weekly digest |
| Reports | No | No | Included within limits |

Everything in the Visitor column is what indexable pages contain. Gated content is never server-rendered
into cached pages (11.5).

### 4.5 Quiz

Inputs: position, body band, partner, pain areas, temperature, firmness preference, motion sensitivity,
edge use, materials or allergies, adjustable base, size, budget, delivery preference, trial importance.
Answers set rubric weights or filters. Output: three picks with reasons templated from score deltas, an
"avoid if" line, offers with disclosure, and an email capture with double opt-in that enrolls the visitor in
price alerts for the picks; marketing enrollment requires the separate checkbox. No AI at request time.
Aggregated answers are stored as demand data with a 90-day retention for un-emailed sessions.

### 4.6 Comparisons and rankings

Comparison page: side-by-side data card, score panel, parsed warranty terms, first-party price history
overlay, native review counts and unweighted averages by sleeper band, sentiment deltas, "choose A if,
choose B if" generated from deltas with citations, offers. Rankings: filtered views over `scores` with the
rationale written from the rubric; "top rated" is the rubric overall (`scores.kind = 'overall'`); ties
broken by `native_review_n desc, observation_n desc, product_id`; every list shows evidence counts and
last-verified dates and an AI-assistance line.

### 4.7 Analytics layer (the unlock)

Per model: rating distribution by criterion; satisfaction by body band and position; durability by
ownership year (headline for reviewers, full for premium); complaint frequency; sentiment by source type
(native, retailer, video, editorial); price history; warranty strength; return experience. Per category:
brand rollups, discount observation index (observed days on sale and real drops, never intent), warranty
index, fiberglass map with public-record sources. Computed by SQL views refreshed on data change.

### 4.8 Alerts and email

Transactional (Resend): verification, review published, follow-up requests, alert triggers, double opt-in.
Marketing (Klaviyo, separate consent): weekly deals digest, sale-event previews, quiz follow-up,
founding-reviewer campaign. Alert types: price below target, price drop percentage, observed-sale detected,
rank change, new model above score, follow-up due. Global Privacy Control honored on marketing.

### 4.9 Brand portal (Phase 5)

Claim flow with domain-email verification and manual approval; response composer with moderation; own-product
aggregated analytics with minimum cell size 5; correction requests; billing when the paid tier activates.
No access to scoring inputs or other brands' raw data.

### 4.10 Admin mission control

Today panel; pipeline runs and dead-letter queue; draft and publish queue with bulk actions and evidence
panel; moderation, verification, brand-response, dispute, and correction queues; entity merge and split
queue; change log with revert per row and per run; sources and quotas; offers, codes, and link health;
cost ledger by task, model, and product; growth panel (Search Console by page type, proposals, losers);
membership metrics when payments exist; rubric versions and rescoring; budgets and kill switches; deletion
requests.

## 5. System architecture

```
   Sources (managed collectors, APIs, site forms)
        |
        v
  +-------------+     +---------------------+     +----------------------+
  | n8n         | --> | source_documents    | --> | observations         |
  | collectors  |     | raw_content (TTL)   |     | (paraphrased, cited, |
  +-------------+     +---------------------+     |  natural-key upsert) |
                                                  +----------+-----------+
  +-------------+     +---------------------+                |
  | site review | --> | reviews (status     | ---------------+  (published reviews are
  | forms (Next)|     | machine), ratings,  |                |   also extracted to
  +-------------+     | evidence            |                |   observations)
                      +---------------------+                v
                                              +--------------------------+
                                              | SQL: dirty-flag scoring  |
                                              | per category, rankings,  |
                                              | index_gate, entitlements |
                                              +-----------+--------------+
                                                          |
                    +-------------------------------------v-----------------------------+
                    | Supabase Postgres: catalog, evidence, reviews, scoring, content,   |
                    | commerce, accounts, ops. pg_cron, pgmq, RLS, change_log triggers, |
                    | public_api views (security_invoker, column whitelists)            |
                    +-----------+--------------------------------------+----------------+
                                |                                      |
                    +-----------v-----------+              +-----------v-----------+
                    | Next.js public site   |              | Next.js /admin and    |
                    | ISR visitor view only |              | gated route handlers  |
                    +-----------------------+              +-----------------------+
                                |
                    +-----------v-----------+   +---------------+   +----------------+
                    | Resend / Klaviyo      |   | Stripe (later)|   | Pinterest, email|
                    +-----------------------+   +---------------+   +----------------+
```

Rules: all state in Postgres; workflows are stateless and idempotent (natural-key upserts); deterministic
logic is SQL or small route handlers with tests; AI calls go through one sub-workflow that reserves budget
first; the site's cached pages contain only visitor data; every write to tracked tables is logged as a
full-row change and reversible.

## 6. Technology stack and tools

| Layer | Choice | Type | Cost now | Cost when live | Why | Replacement |
|---|---|---|---|---|---|---|
| Database, auth, storage, cron, queues | Supabase | Low-code | Free during build (n8n keep-alive pings both projects because Free pauses pg_cron after 7 idle days) | Pro $25/mo from first real user data (Phase 3): backups, no pause, 100 GB storage | Plain Postgres, RLS, MCP for agents | Managed Postgres with same schema |
| Orchestration | n8n self-hosted (Docker locally during build; Railway or VPS in production) | No-code | $0 | $5 to $12/mo | Visual, native Postgres and AI nodes, error workflows, license covers internal use | pg_cron plus edge functions for hot paths; queue mode |
| Site and admin | Next.js on Vercel | Code (AI-maintained) | Hobby during build only while nothing public and commercial exists | Pro $20/mo from the moment batch one is indexable (Vercel Hobby forbids commercial use) | ISR, structured data, admin in one deploy | Same framework elsewhere |
| AI | Anthropic direct: Haiku 4.5 (extraction, classification, moderation, photo checks; Batch API and caching), Sonnet 5.5 (summaries, narration, QA); OpenAI small model for independent QA when a key exists | Config | Metered, capped | $20 to $150/mo | No gateway fee; registry makes swaps a row change | Gateway if providers exceed three |
| Near-duplicate detection and search | Postgres full-text plus pg_trgm similarity | Built-in | $0 | $0 | No embedding vendor or storage cost at launch | pgvector with a small embedding model at growth stage |
| Collection | Apify actors (Amazon and retailer review pages, incremental), Firecrawl (manufacturer and editorial pages, refresh by content hash), YouTube Data API (counts only persist), Wayfair feed, Brave Search for discovery (results not stored) | Low-code | Free credits | $0 to $60/mo with per-run caps | Managed, no scraper maintenance | Second provider |
| Price tracking | n8n HTTP fetch with a stored per-merchant extractor (CSS or JSON path), 3 runs per week, daily during sale windows; no Firecrawl credits | No-code | $0 | $0 | Cheapest path to first-party price history | Retailer APIs where terms allow |
| Email | Resend (transactional), Klaviyo (marketing; free to 250 profiles, then about $20 to $45/mo) | No-code | $0 | $0 to $65/mo | Good APIs | Postmark, SES |
| Payments (Phase 6) | Stripe Checkout, Billing, Customer Portal, Tax | No-code checkout | $0 | Fees only | Instant onboarding, US-only | Polar or Paddle for Merchant of Record |
| Monitoring | Sentry free, Better Stack free, n8n error workflows, cost ledger | No-code | $0 | $0 to $26 | Enough for one operator | Paid tiers |
| Social | Pinterest API (app approval required), email; Instagram, X, YouTube uploads deferred to Phase 7 with their own prerequisites and costs | Low-code | $0 | $0 | Autopilot from the database | Scheduler tools |
| Analytics | Search Console API, Plausible or Vercel Analytics, affiliate network reports, into Supabase | Low-code | $0 | $0 to $9 | One dashboard | Same |
| Repo and CI | GitHub, GitHub Actions (migrations up and down, pgTAP, Playwright, structured-data validation, gating-leak test, prompt evals) | Code | $0 | $0 | Everything gated by tests | Same |

Development-time tooling: Supabase plugin and MCP against the dev project; `product-intel-conventions` skill
created from this plan; Stripe plugin in Phase 6; Sentry and Playwright from Phase 2. No community-tier
plugins.

## 7. Data model

Schemas: `catalog`, `evidence`, `reviews`, `scoring`, `content`, `commerce`, `accounts`, `ops`. Every table
has `id uuid primary key`, `created_at`, `updated_at`. Tracked tables carry full-row change-log triggers.
Money is stored as `microusd bigint`. Only the `public_api` schema is exposed through PostgREST.

### 7.1 catalog
- `categories` (family, slug, name, status, config jsonb: max_products, max_sources_per_product,
  daily_ai_budget_microusd, refresh_interval_days, min_observations default 20, min_sources default 2,
  min_native_reviews default 5, min_price_history_days default 30, min_unique_ratio default 0.5)
- `brands` (name, slug, website, parent_company, affiliate_program_id, fiberglass_status,
  fiberglass_source_document_id)
- `brand_public_records` (brand_id, kind ftc|recall|litigation|law, summary, source_document_id, occurred_at)
- `products` (category_id, brand_id, canonical_name, slug unique, lifecycle_status, published_at,
  score_dirty bool, next_refresh_at, msrp_microusd, image_path)
- `product_variants` (product_id, name, firmness, size, model_number, upc, gtin, sku_map jsonb)
- `product_identifiers` (variant_id, type, value, source_document_id; unique(type, value))
- `product_aliases` (product_id, alias, source_document_id; unique(alias))
- `entity_candidates` (raw_name, raw_model, source_document_id, proposed_product_id, confidence, decision,
  decided_by, decided_at)
- `attributes` (category_id, key, name, unit, datatype, is_objective, is_required, normalization jsonb)
- `spec_values` (product_id, variant_id nullable, attribute_id, value_text, value_num, unit, normalized_num,
  source_document_id, confidence, collected_at, verified_at, run_id, is_canonical; partial unique
  (product_id, attribute_id) where is_canonical)
- `warranty_terms` (product_id, years, prorated_after_years, sag_threshold_inches, exclusions jsonb,
  trial_nights, return_fee_microusd, return_conditions, source_document_id, verified_at; unique(product_id))
- `certifications` (product_id or brand_id, scheme, certificate_id, verified_at, source_url)

### 7.2 evidence
- `sources` (key, name, type, license_class, retention_days, allow_excerpt bool, store_title bool,
  store_author bool, rate_limit, cost_per_call_microusd, credentials_ref). YouTube: retention_days 30,
  allow_excerpt false, store_title false, store_author false.
- `source_documents` (source_id, external_id, product_id nullable, url, title nullable, fetched_at,
  content_hash, expires_at, provider_run_id; unique(source_id, external_id)). One row per third-party
  review, video, comment, or page.
- `raw_content` (source_document_id, body, mime, expires_at); nightly purge.
- `observations` (product_id, criterion_key nullable, source_document_id nullable, review_id nullable
  (check: exactly one set), kind, polarity, strength 1 to 3, excerpt varchar(400) with a CHECK of 25 words
  or fewer and null when the source disallows excerpts, pointer jsonb limited to url and source-local id,
  pointer_hash, sleeper_context jsonb, extracted_by_model, prompt_version, run_id, superseded_by,
  expires_at nullable; unique(coalesce(source_document_id, review_id), criterion_key, pointer_hash))
- `price_points` (offer_id, price_microusd, list_price_microusd, observed_at, on_sale bool, event_tag;
  unique(offer_id, observed_at::date))

### 7.3 reviews
- `reviews` (product_id, variant_id, user_id, purchase_month, price_paid_microusd, retailer, sleeper jsonb,
  overall smallint, likes, dislikes, comment, status enum, moderation_result jsonb, verification_result
  jsonb, verification_tier smallint, material_connection bool, material_connection_text, incentive_disclosed
  bool, published_at, ip_hash, device_hash; partial unique (user_id, product_id) where status not in
  ('rejected','removed'))
- `review_ratings` (review_id, criterion_key references rubric criteria keys, value smallint 1 to 5;
  unique(review_id, criterion_key))
- `review_updates` (review_id, at_months, durability, sag_observed, still_recommend, comment, status,
  published_at)
- `review_evidence` (review_id, kind bed_photo|law_label|receipt|serial, storage_path nullable, sha256,
  verified bool, verified_by, verified_at, deleted_at). Receipts and law labels are deleted 30 days after
  verification; the hash and result remain.
- `moderation_events` (subject_kind review|update|brand_response, subject_id, actor_type, action, reason,
  model, created_at)
- `brand_claims` (brand_id, user_id, email_domain, status, verified_at)
- `brand_responses` (review_id, brand_user_id, body plain text, status, published_at)
- `disputes` (review_id, raised_by, ground enum, evidence, status, resolution, resolved_at)
- `corrections` (subject_kind, subject_id, brand_user_id, claim, source_url, status, resolved_at)
- `consents` (user_id, kind terms|privacy|content_license|marketing|age, version, accepted_at)

### 7.4 scoring
- `rubrics` (category_id, version, status, weights jsonb, notes)
- `rubric_criteria` (rubric_id, key, name, type objective|subjective|owner, weight, normalization jsonb,
  evidence_floor int, prior_weight int default 10)
- `assessments` (product_id, rubric_id, criterion_key, score, rationale, evidence_count, confidence, model,
  prompt_version, run_id, is_current; partial unique(product_id, rubric_id, criterion_key) where is_current)
- `owner_aggregates` (product_id, criterion_key, n_raw, n_weighted, mean_raw, mean_weighted, shrunk_mean,
  n_tier2, by_band jsonb, by_position jsonb, computed_at, run_id, is_current; partial unique as above)
- `scores` (product_id, rubric_id, criterion_key nullable, kind objective|owner|subjective|overall, value,
  confidence, coverage, computed_at, run_id, is_current; partial unique(product_id, rubric_id,
  coalesce(criterion_key,''), kind) where is_current). History retained for the last 30 runs per product.
- `rankings` (category_id, rubric_id, modifier_key nullable, product_id, rank, score, native_review_n,
  observation_n, computed_at, run_id, is_current; partial unique(category_id, rubric_id,
  coalesce(modifier_key,''), product_id) where is_current)

### 7.5 content
- `content_blocks` (subject_kind, subject_id, block_type, version, status, body jsonb, input_hash,
  source_count, generated_by, prompt_version, qa_report jsonb, approved_by, published_at, is_current)
- `content_citations` (block_id, sentence_idx, target_kind observation|review|spec_value|warranty_term|
  assessment|price_point|public_record, target_id; trigger validates the target exists)
- `pages` (route unique, page_type, product_id, category_id, comparison_pair_id (check: exactly one set),
  status, indexable bool, gate_report jsonb, last_built_at, last_verified_at)
- `comparison_pairs` (product_a, product_b, demand_signal, status; unique(least, greatest))
- `data_studies` (slug, title, query_ref, dataset_path, published_at)

### 7.6 commerce
- `merchants` (name, network, program_id, cookie_days, rate_note, status, deep_link_policy)
- `affiliate_offers` (product_id, variant_id, merchant_id, url_template with {subid}, price_microusd,
  currency, availability, price_as_of, last_verified_at, active)
- `promo_codes` (offer_id, code, authorized_by, valid_from, valid_to, exclusive bool, status)
- `clicks` (id used as subid, offer_id, page_route, user_id nullable, session_hash, at); retained 13 months
- `conversions` (network, offer_id, click_id nullable, amount_microusd, commission_microusd, status,
  occurred_at, reversed_at)

### 7.7 accounts
- `profiles` (user_id, display_name, role, brand_id nullable, preferences jsonb). Users may update only
  display_name and preferences; role and brand_id are service-role only.
- `subscribers` (email unique, source, confirmed_at, unsubscribed_at, marketing_consent bool, user_id
  nullable)
- `saved_items`, `saved_comparisons`, `user_weightings`
- `alerts` (user_id nullable, subscriber_id nullable (check: one set), type, target_id, threshold, active),
  `alert_events`
- `quiz_sessions` (user_id nullable, subscriber_id nullable, answers jsonb, results jsonb, expires_at)
- `plans`, `subscriptions`, `entitlements` (user_id, key, value, source review|plan|manual, source_id,
  expires_at), `payment_customers`, `payment_events` (unique(processor, event_id)), `usage`
- `deletion_requests` (user_id, requested_at, completed_at, log jsonb)

### 7.8 ops
- `pipeline_runs`, `pipeline_steps` (input_hash, output_hash), `ai_calls` (run_id, step_id, product_id
  nullable, task, provider, model, input_tokens, output_tokens, cached_tokens, cost_microusd, latency_ms,
  prompt_version), `budgets` (scope, period, limit_microusd, spent_microusd, reserved_microusd, kill_switch),
  `change_log` (table_name, row_id, op insert|update|delete, old_row jsonb, new_row jsonb, actor_type,
  actor_id, reason, run_id, reverted_by; admin-only RLS), `models`, `prompt_versions`,
  `search_console_daily` (route, query, impressions, clicks, position, date), `growth_proposals`,
  `settings`

Indexes on all foreign keys; composite indexes as listed in the unique constraints plus
`observations(product_id, criterion_key)`, `reviews(product_id, status)`, `search_console_daily(route,
date)`. RLS on all `accounts` and `reviews` tables and on `change_log`.

## 8. Backend design

### 8.1 Layout and exposure
One Supabase project per environment. Migrations in `supabase/migrations/` applied by GitHub Actions with
an up-and-down test. Only the `public_api` schema is exposed through PostgREST. Every `public_api` view is
created with `security_invoker = true`, selects a whitelist of columns, filters to published rows, and is
granted only to `anon` and `authenticated`. Write paths are RPC functions and route handlers running with
the service role server-side.

### 8.2 SQL functions (deterministic core, each with pgTAP fixtures)

- `normalize_spec(attribute_id, value)`: applies the attribute's normalization (min-max within the category
  computed once per scoring run, log, or thresholds, with direction) and returns 0 to 10.
- `aggregate_owner_ratings(product_id, run_id)`: per criterion over published `review_ratings`:
  weights w = 1.0 for Tier 2, 0.7 for Tier 1, 0 for material-connection reviews;
  `n_weighted = sum(w)`, `mean_weighted = sum(w * value) / n_weighted`;
  prior = category weighted mean for the criterion frozen at the start of the run (fallback: 3.5);
  `shrunk_mean = (n_weighted / (n_weighted + m)) * mean_weighted + (m / (n_weighted + m)) * prior` with
  `m = rubric_criteria.prior_weight` (default 10). Also stores `n_raw` and `mean_raw` (unweighted, excluding
  material-connection reviews) for display and AggregateRating.
- `compute_scores(product_id, rubric_id, run_id)`: objective criteria from `normalize_spec` on canonical
  specs; owner criteria = `shrunk_mean * 2` (1 to 5 scale to 0 to 10); subjective criteria from the current
  assessment when `evidence_count >= evidence_floor`, else unscored. `coverage = sum(weight of scored
  criteria) / sum(all weights)`. `overall = sum(weight * score) / sum(weight of scored) * (1 - penalty)` with
  `penalty = 0.15 * least(1, greatest(0, 0.70 - coverage) / 0.70)`. If `coverage < 0.5` the product is
  scored but excluded from rankings and flagged. `confidence = coverage * least(1, (native_review_n + 0.25 *
  observation_n) / 40)`. Writes `scores` with `is_current` maintenance.
- `compute_rankings(category_id, rubric_id, modifier_key, run_id)`: orders by `overall desc,
  native_review_n desc, observation_n desc, product_id`, excludes coverage under 0.5, writes `rankings`.
- `personal_ranking(user_id, category_id, weights)`: same formula with user weights at request time.
- `index_gate(product_id)`: passes only when all hold: (a) data card complete: every `attributes.is_required`
  has a canonical `spec_values` row, `warranty_terms` has `years` and `trial_nights`, and a `price_points`
  row exists within 30 days; (b) owned signal: at least `min_price_history_days` of first-party price
  history or at least 3 published native reviews; (c) evidence: at least `min_observations` non-superseded,
  non-expired observations across at least `min_sources` distinct sources, or at least `min_native_reviews`
  published native reviews; (d) unique-content ratio at or above `min_unique_ratio`. Writes `pages.indexable`
  and `gate_report`. This is the only index rule; `CLAUDE.md` states the same rule.
- `publish_review(review_id)`: row lock; requires clean moderation and verification results; sets
  published; inserts the `entitlements` row (`source = review`, `expires_at = published_at + 12 months`);
  enqueues `extract` for the review; enqueues scoring dirty flag. `remove_review` and `reject_review`
  revoke the entitlement.
- `effective_entitlements(user_id)`: merges active subscription plan, non-expired review entitlements, and
  manual grants.
- `budget_check(scope, estimated_microusd)`: atomically adds to `reserved_microusd` and refuses when
  `spent + reserved >= limit`; `budget_settle(reservation_id, actual_microusd)` on completion.
- `revert_run(run_id)` and `revert_change(change_id)`: reverse-chronological application of `change_log`
  rows through the same RPCs so triggers fire; marks affected products dirty; enqueues rebuild.
- `delete_user(user_id)`: anonymizes reviews (ratings kept under the content license, text and evidence
  removed), purges storage objects and change-log rows containing the user's data, calls Resend, Klaviyo,
  and Stripe deletion APIs, logs to `deletion_requests`.

Scoring orchestration: row triggers on specs, observations, reviews, and assessments only set
`products.score_dirty`. A pg_cron job every 10 minutes takes `pg_advisory_xact_lock(hashtext(category_id))`,
freezes category statistics, rescores all dirty products in the category under one `run_id`, then
recomputes rankings once. A rubric or normalization change marks the whole category dirty.

### 8.3 Scheduled jobs (pg_cron)
Every 10 minutes: category rescoring. Nightly: purge expired `raw_content`, `source_documents`, and
`observations`; receipt and label deletion at 30 days after verification; price event detection;
entitlement expiry; quiz session expiry; hash rotation. Weekly: refresh enqueue; link and code health;
growth proposals; digest build. Monthly: rubric drift report; cost report; DMCA renewal reminder check.

### 8.4 Queues (pgmq) and idempotency
Queues: `collect`, `extract`, `assess`, `build_page`, `qa`, `publish`, `moderate`, `verify`, `notify`,
`social`. Messages carry entity id, run id, and attempt count; three retries with backoff for transport
errors, none for validation failures; dead-letter queue in admin. Every write step is an upsert on the
natural keys in section 7, so retries and re-collection never double-count.

### 8.5 Route handlers and edge functions
`submit_review`, `submit_update`, `brand_claim`, `brand_response` (route handlers with service role);
`stripe_webhook` (edge function, signature check, idempotent by event id); `/go/[click_id]` (Next route
handler: logs click, 302 to `url_template` with subid, X-Robots-Tag noindex); `search_console_sync`;
`quiz_result`; `gated/*` route handlers that call `effective_entitlements` and return gated fragments.

### 8.6 Storage
Buckets: `review-evidence` (private, never displayed; receipts, serials, law labels; objects deleted 30 days
after verification), `review-photos` (public; re-encoded 1600 px WebP, EXIF stripped, produced by W07; the
only review images shown; photos with people are rejected), `product-images` (public), `datasets` (public
CSV exports).

### 8.7 Search and duplicates
Postgres full-text over products, brands, and content; pg_trgm similarity for near-duplicate review
detection. pgvector deferred to growth stage.

### 8.8 Security
RLS on every user-facing table; column-level grants so users update only `display_name` and
`preferences`; service role only inside n8n, route handlers, and edge functions; secrets in Supabase Vault,
n8n credentials, and Vercel environment; signed webhooks; rate limits on submission and quiz; WAF rules on
abuse. See section 17.

## 9. Workflow catalog

Every workflow writes `pipeline_runs` and `pipeline_steps`, calls `budget_check` before metered steps and
`budget_settle` after, and upserts on natural keys.

| ID | Workflow | Trigger | Steps | Outputs | Budget scope |
|---|---|---|---|---|---|
| W01 | Catalog ingest | Weekly cron; manual | Fetch brand lineup and product pages (Firecrawl, skip unchanged by content hash), extract specs and warranty terms, resolve entities (W21), write `spec_values` and `warranty_terms` with provenance | Products, variants, specs, warranties | collection, ai_extract |
| W02 | Price tracker | 3 runs per week; daily inside sale windows | HTTP fetch per merchant with stored extractor; write `price_points`; run price event detection | First-party price history, sale events | collection (tiny) |
| W03 | Third-party review collection | Queue `collect`; monthly per product, incremental where the actor supports "newer than" | Managed actor or API per source policy; write `source_documents` (unique external id) and `raw_content` with TTL; YouTube stores ids and counts only | Raw content | collection (per-run and weekly caps) |
| W04 | Observation extraction | Queue `extract` (raw content or published native review) | Batch to Haiku with the extraction schema; validate; upsert observations with excerpt rules per source; mark superseded | Observations | ai_extract |
| W05 | Sentiment summary | When new observations for a product exceed max(10 percent, 5) since `content_blocks.input_hash`, with a 7-day cooldown | Regenerate only criteria whose observation set changed (Sonnet), write `content_citations`, QA (W10) | Summary blocks | ai_write |
| W06 | Moderation | Queue `moderate` on review, update, or brand response | Haiku classifier; trigram duplicate check; auto-clean or human queue; never keyed on rating | Moderation result | ai_moderate |
| W07 | Verification | Queue `verify` | Photo check (image present, no people, not a stock image) with a vision-capable model; receipt parse extracting only date, retailer, product line; re-encode photo to `review-photos`; set tier; schedule evidence deletion | Verification result | ai_extract |
| W08 | Scoring | pg_cron every 10 minutes on dirty products | `aggregate_owner_ratings`, `compute_scores`, `compute_rankings` per category under lock | Scores, rankings | none |
| W09 | Page build | Queue `build_page` | Assemble blocks; comparison narration with citations; run `index_gate`; write `pages` and drafts | Pages, drafts | ai_write |
| W10 | QA | Queue `qa` | Independent model checks each sentence against its citation rows; rejects blocks with uncited sentences | qa_report | ai_qa |
| W11 | Publish | Admin approval or per-category auto-publish setting | Set published, ISR revalidation, sitemap regeneration, internal-link recompute, IndexNow for Bing | Live pages | none |
| W12 | Refresh | Weekly cron | Content-hash checks on manufacturer pages, incremental review and video checks, re-run only changed steps, refresh YouTube documents inside the 30-day window, detect successor models | Updated data | collection, ai_extract |
| W13 | Alerts and email | Nightly and event-driven | Batch `alert_events` per user or subscriber; Resend; weekly digest via Klaviyo to consented profiles | Emails | email |
| W14 | Review follow-ups | Daily cron | Requests at 6, 12, 24 months; updates through W06; extend unlock on publish | Updates | email |
| W15 | Search Console ingest | Daily cron | Pull by route and query into `search_console_daily` | Metrics | none |
| W16 | Growth proposals | Weekly cron | Propose comparison pairs with impressions but no page; modifier pages with query evidence; noindex for zero-impression thin pages after 8 weeks; flag models with clicks but no price data | Proposals | none |
| W17 | Social autopilot (Phase 4: Pinterest and email only) | Daily cron | Select data events; render varied card templates; 1 to 2 pins per day linking to site pages; captions carry incentive and affiliate disclosures where applicable | Posts | ai_write |
| W18 | Offer, link, and code health | Weekly cron | HEAD checks, redirect targets, availability; validate promo codes against network feeds and validity windows; deactivate failures | Offer and code status | none |
| W19 | Budget reconciliation | Every 15 minutes | Settle batch results, reconcile provider spend with `budgets`, alert on drift; kill switch is flipped by `budget_check` itself | Reconciliation | none |
| W20 | Daily health report | Daily cron | Runs, failures, drafts, moderation and verification backlogs, spend, expiring documents, growth summary | Report | email |
| W21 | Entity resolution | After W01 and W03 | Blocking on brand and model tokens; identifier match; AI adjudication for ambiguous; merge and split queue | Decisions | ai_extract |
| W22 | Data studies | Monthly cron | Run study queries, render charts and CSV, publish, enqueue social and journalist-pitch drafts | Studies | ai_write |
| W23 | Deletion requests | Event | Run `delete_user`; call provider deletion APIs; confirm to requester | Deletion log | none |
| W24 | Keep-alive and backups | Daily | Ping Supabase Free projects during build; weekly `pg_dump` to storage | Uptime, backups | none |

## 10. AI task registry

| Task | Model | Mode | Output schema | Cap per call | Eval set (operator labels) |
|---|---|---|---|---|---|
| spec_extraction | Haiku 4.5 | Batch, cached | attribute key, value, unit, confidence | $0.02 | 20 pages, 2 hours |
| warranty_parse | Haiku 4.5 | Batch | years, prorated_after, sag_threshold, fees, exclusions | $0.02 | 15 documents, 1 hour |
| observation_extract | Haiku 4.5 | Batch | criterion, polarity, strength, sleeper_context, excerpt (25 words max or null) | $0.01 | 50 reviews, 3 hours |
| entity_adjudicate | Haiku 4.5 | Online | same_product, variant_of, confidence | $0.01 | 30 pairs, 1 hour |
| review_moderate | Haiku 4.5 | Online | flags[], severity, action | $0.005 | 40 reviews, 2 hours |
| photo_verify | Haiku 4.5 (vision) | Online | has_bed, has_person, is_stock_like, confidence | $0.01 | 30 photos, 1 hour |
| receipt_parse | Haiku 4.5 (vision) | Online | date, retailer, product_line only | $0.01 | 15 receipts, 1 hour |
| sentiment_summary | Sonnet 5.5 | Online | summary sentences each with citation ids, counts | $0.06 | 15 products, 3 hours |
| comparison_narrate | Sonnet 5.5 | Online | choose-A-if and choose-B-if bullets with citations | $0.04 | 15 pairs, 2 hours |
| ranking_rationale | Sonnet 5.5 | Online | rationale with citations | $0.05 | 10 lists, 1 hour |
| qa_review | OpenAI small model (Haiku until a key exists) | Online | unsupported_sentences[], score | $0.03 | 20 drafts, 2 hours |
| social_copy | Haiku 4.5 | Online | platform text with required disclosures | $0.005 | 20 events, 1 hour |
| journalist_pitch | Sonnet 5.5 | Online | draft reply with citations | $0.03 | 10, 1 hour |

Rules: structured outputs validated against schema; invalid output retried once then failed; prompts
versioned in `/prompts` and mirrored to `prompt_versions`; every call logged to `ai_calls` with run, step,
and product; per-scope daily budgets with reservation and settlement; Batch API and caching for anything
not latency-sensitive; evals gate any prompt or model change in CI. Expected typical cost per model in
Phase 1 with batch pricing: $0.80 to $1.20 for 200 collected reviews plus specs and summaries; acceptance
ceiling $2.50. Expected AI spend at launch scale (30 models, monthly refresh): under $40 per month.

## 11. Frontend design

### 11.1 Stack and rendering
Next.js App Router, TypeScript, Tailwind. Indexable routes are rendered with visitor-only data from
`public_api` views and cached with ISR; on-demand revalidation from W11. Gated content is never
server-rendered into a cached page: gated sections are client components that call authenticated
`gated/*` route handlers, which run `effective_entitlements`. Rule: no client-side fetching for indexable
content; gated content is only ever client-fetched. CI runs an anonymous fetch of every indexable route and
fails on any gated marker.

### 11.2 Routes
`app/(site)/…` public; `app/(account)/…` behind auth; `app/admin/…` behind role check; `app/api/go/[click_id]`,
`app/api/gated/*`, `app/api/submit/*`.

### 11.3 Core components
DataCard (specs, warranty, certifications, sources); ScorePanel (overall, per-criterion bars, confidence,
coverage, evidence counts, rubric version, AI-assistance line); ReviewList (tier badge, sleeper context,
incentive disclosure, material-connection disclosure, brand response); SentimentSummary (per criterion,
total observation count and number of sources public; per-source breakdown gated; "Sources" row visible and
clickable; AI-assistance line); PriceHistory (30 days for visitors); OfferBlock (offers, network-issued
codes, price-as-of, disclosure adjacent, links through `/go/` with rel="sponsored"); ComparisonTable;
QuizFlow; UnlockPrompt; AlertButton; Methodology and Author blocks on every ranking and comparison; a
sitewide AI-assistance statement on `/methodology/`.

### 11.4 Structured data
Product; Review with author = Person (the operator) and publisher = Organization, reviewBody = the visible
editorial verdict block, reviewRating = the rubric overall; positiveNotes and negativeNotes only from
criteria with native evidence at or above the evidence floor, visible on the page, at least two each;
AggregateRating = unweighted mean and count of published native reviews excluding material-connection
reviews, shown only when the count is 5 or more; BreadcrumbList; Organization; Person; ItemList on
rankings. No gated data appears in structured data or in the cached HTML.

### 11.5 Gating UX
Gated elements render a skeleton with real headline numbers and a one-line unlock prompt. The client never
decides access; the route handler does.

### 11.6 Performance and accessibility
Lighthouse budgets in CI; semantic HTML; keyboard-accessible forms; alt text from data.

### 11.7 Admin
Same app, server actions calling RPCs, bulk actions, evidence panel, change-log diff and revert.

## 12. Entitlements and payments

Entitlement sources: `review` (one row per published review or follow-up, 12 months), `plan` (active
subscription), `manual`. Resolution in `effective_entitlements`. Revocation on review removal. Premium-only
assets are listed in 4.4.

Payments (Phase 6): Stripe Checkout, Billing, Customer Portal, Tax; idempotent webhooks; nightly
reconciliation; trial and auto-renewal disclosure at checkout and in email; one-click cancel. Merchant of
Record only if international sales begin.

## 13. Data acquisition and legal posture

| Source | Access | Stored | Retention | Rule |
|---|---|---|---|---|
| Manufacturer pages, spec sheets, warranty PDFs | Firecrawl, robots-respecting, hash-skipped | Facts, parsed terms, links | Indefinite | Facts only; no marketing text or images |
| Brand and retailer prices | HTTP fetch with stored extractor; Wayfair feed | Price points | Indefinite | Observed facts; Best Buy API excluded from price history by its terms |
| Native reviews | Site form | Full, with consents | Until deletion request | User license covers display, derivative summaries, social reposting with attribution, aggregated licensing |
| Collected third-party reviews (Amazon and retailer pages via Apify; YouTube API) | Managed providers, APIs | Observations, excerpts where allowed, pointers without reviewer identity; raw purged in 30 days | Raw 30 days; YouTube documents and any text 30 days; observations from other sources indefinite | Analysis only; never republished; never native; Amazon Associates excluded; docs/12 deliberately overrides the research's "not usable" rating for Amazon actors and accepts the risk |
| Walmart Affiliate API | Only after the affiliate account exists (Phase 5) | Aggregates only, shown on pages carrying a Walmart offer | Per terms | Advertising-purpose limitation |
| Editorial reviews | Brave discovery (results not stored), direct fetch | Score, one-line quote, link | Indefinite | Fair-use citation |
| Certification directories | Lookup | Certificate facts | Indefinite | Public directories |
| CPSC, FTC, state law | Public records | Facts with a public-record source row for every claim shown | Indefinite | Every FTC, recall, litigation, or fiberglass statement on a page cites `brand_public_records` |
| Reddit | Discovery | URL only, no titles | Indefinite | Linking only |
| Scoped web research (OpenAI or Claude) | API, sparse | Our paraphrase plus URLs | Indefinite | Output owned; citations displayed |

Platform compliance: Terms (content license as above, 18+, dispute and correction processes, repeat-infringer
policy), Privacy Policy (retention schedule, AI processing of evidence, licensing disclosure, GPC, cookie
notice), Review Policy (collection, tiers, moderation categories, incentive, brand responses, dispute
grounds, published outcome counts), Affiliate Disclosure, DMCA agent registered and renewed every three
years with agent details on `/dmca/`. First-party statements (summaries, brand hubs, deal pages) are not
covered by Section 230; they are sourced, phrased as observations, and correctable through the brand
channel. The founding-reviewer giveaway is a sweepstakes with official rules, no-purchase entry, eligibility
terms, and copy that expects no sentiment.

## 14. SEO and growth engine

1. Page-type order: model pages and brand hubs first (user-platform query class), then comparisons, then
   modifier rankings, then data studies and tools, head terms last.
2. Batch one: 30 to 40 models that pass `index_gate` including the owned-signal requirement; expansion only
   when 70 percent of a batch is indexed and impressions rise four weeks running; automatic noindex for
   zero-impression thin pages after eight weeks. There is no URL-per-month target.
3. On-page automation: structured data per 11.4, internal links computed on publish, templated titles with a
   unique headline number, visible last-verified dates, methodology and author links, disclosures,
   AI-assistance lines.
4. Links and citations: monthly data studies with CSV; journalist-request workflow; free badges to brands
   that earned ranks with optional nofollow links; embeddable tools; explicit data-use permission.
5. Measurement loop: W15 and W16 into the growth panel; weekly operator approval of proposals.
6. AI answer engines: quantified claims, quotable summary blocks, complete structured data, freshness.
7. Sitemaps split by page type; IndexNow for Bing; Google through sitemaps and links.

## 15. Social autopilot

Phase 4 scope: Pinterest (after API app approval) at 1 to 2 pins per day with varied templates, linking to
site pages, never to `/go/`; email flows. Captions that quote native reviews carry both the reviewer
incentive disclosure and, where a link is affiliate, the affiliate disclosure. Auto-post from day one with a
kill switch and a daily cap.

Phase 7 additions, each with prerequisites and cost lines: Instagram (Facebook Page, app review for content
publishing, business verification); X (free tier covers 1 to 3 posts per day; automated-account label);
YouTube uploads (API compliance audit before automated uploads; until then manual; each video carries unique
data and on-screen attribution to satisfy inauthentic-content policy); TikTok (posts private until the app
is audited). No templated mass video production.

## 16. Operations and observability

Mission control is the operator's daily surface. Alerts: budget breaches, dead-letter growth, moderation or
verification backlog over 24 hours, failed sends, uptime, Search Console drops over 30 percent week over
week, DMCA notices. Weekly routine under four hours: bulk-clear drafts, flags, proposals, journalist replies;
review costs and losers. Runbooks in `docs/runbooks/`: pause pipelines; revert a run; re-run a product;
rotate a credential; restore from backup; brand dispute; DMCA notice; deletion request; de-index playbook if
a collection source is lost. Backups: Supabase Pro daily plus weekly dump.

## 17. Security, privacy, and compliance

Secrets only in Vault, n8n credentials, and Vercel environment; least-privilege keys with vendor spend caps;
RLS and column grants; service role server-side only; signed webhooks; rate limits; EXIF stripping and
re-encoding at ingest; private evidence never displayed; no PII in logs; salted, rotating IP and device
hashes; `/security-review` before every merge; monthly dependency updates; Sentry; WAF rules.

Retention schedule: `ip_hash` and `device_hash` 12 months; `clicks` 13 months; `quiz_sessions` 90 days unless
emailed; `review_evidence` receipts and labels 30 days after verification; raw third-party content 30 days;
YouTube-derived documents 30 days; `alert_events` 12 months; `change_log` rows containing user data purged on
deletion. Marketing consent is a separate checkbox; Global Privacy Control honored; cookie notice present;
18+ attestation on the review form; photos with people rejected.

## 18. Cost model and unit economics

| Stage | Fixed per month | Metered | Notes |
|---|---|---|---|
| Phase 0 one-time | about $500 | | Domains (.com and .ai), trademark filing, DMCA registration |
| Build (Phases 0 to 1) | $0 | AI under $20; collection within free credits with caps | Supabase Free with keep-alive, n8n in Docker, no public site |
| Public launch (batch one indexable, Phase 2) | Vercel Pro $20 | AI $20 to $40 | Commercial-use rule triggers Pro at indexing |
| Real user data (Phase 3) | plus Supabase Pro $25, n8n host $5 to $12, Klaviyo $0 to $45 | AI $30 to $60, collection $0 to $60 | About $50 to $200 all-in |
| Growth (500 models, 10k reviews) | plus Supabase compute $10 to $60 | AI $100 to $300, collection $60 to $200, email $20 to $65 | About $250 to $700 |
| Payments live | Stripe fees only | | 2.9% + 30c, +0.7%, +0.5% |

Unit economics (conservative, per review): 13,000 monthly sessions, 10 to 15 percent click-out, 1.5 to 2
percent conversion, $55 to $70 blended commission after sub-affiliate cuts, 25 to 30 percent reversal, 120 to
365 day lag. Result: about $1,500 to $2,500 per month in settled commissions at that traffic. Premium at 30
to 100 members adds $200 to $900 per month. These figures set the revenue-stream triggers in section 3 and
the KPI targets in section 22.

## 19. Build plan

Effort in operator-days with AI-assisted development; the operator approves rather than codes. Test strategy
applies to every phase: pgTAP for every SQL function and every RLS policy under anon, reviewer, brand, and
admin roles; migration up and down in CI; prompt eval harness gating registry changes; structured-data
validation; the gating-leak test; Playwright for user flows; Lighthouse budgets.

### Phase 0: Foundation (7 days, one-time costs about $500)
Register ratemybed.com and .ai; claim handles; file trademark; DMCA agent registration; Supabase dev project
and plugin; repo structure; full schema migration; seed categories, attributes, rubric v1.0; Supabase Auth
with `profiles`, roles, RLS policies, and the operator account; n8n in Docker; Anthropic key with a $20 cap;
`product-intel-conventions` skill from this plan; drafts of Terms, Privacy, Review Policy, Disclosure, DMCA
page, sweepstakes rules for operator review.
Acceptance: migrations apply and roll back in CI; pgTAP passes for `index_gate`, `aggregate_owner_ratings`,
`compute_scores`, `publish_review`, `budget_check`, purge jobs, and all RLS policies; conventions skill loads.

### Phase 1: Catalog and intelligence pipeline (25 days)
W01, W21, W02, W03 (Apify, YouTube, editorial; Walmart deferred), W04, W05, W08, W10, W19, W20, W24 for
30 mattress models and 12 brands.
Acceptance: 30 models with complete data cards; each with 20 or more observations from 2 or more sources;
price history accumulating; scores and rankings reproducible from a run id; typical cost per model $0.80 to
$1.20, ceiling $2.50; zero uncited sentences in QA on a sample of 10 summaries; YouTube purge test passes.

### Phase 2: Site and admin (20 days; Vercel Pro at indexing)
Model pages, brand hubs, methodology, review policy, author, disclosure, DMCA, privacy, terms pages;
components in 11.3; structured data per 11.4; admin panels for drafts, moderation, entities, change log,
costs, growth; W09, W11; sitemaps; Sentry; uptime; Playwright and gating-leak tests.
Acceptance: batch one (30 to 40 models passing `index_gate` with the owned-signal requirement) approved and
live; Lighthouse over 90; structured data validates; anonymous fetch contains no gated markers.

### Phase 3: Review platform (15 days; Supabase Pro at first real user data)
Reviewer auth (magic link, Google); submission flow with evidence; W06, W07; `publish_review` and
entitlements; reviewer disputes; follow-ups W14; founding-reviewer sweepstakes with rules; deletion flow
W23.
Acceptance: clean review published within 10 minutes; unlock applies at publication; AggregateRating only at
5 or more; evidence deletion job verified; deletion request completes end to end.

### Phase 4: Growth engine (12 days)
W15, W16, W12, W13, W18, W22 first study, W17 (Pinterest and email), Klaviyo flows with consent; comparison
pages and rankings; quiz.
Acceptance: growth panel live; first study with CSV; alerts firing on observed price events; first
comparison batch approved.

### Phase 5: Affiliate activation and brand portal (ongoing from month 2)
Apply to Sovrn, FlexOffers, Impact, Awin, CJ and brand programs; offers and network-issued codes loaded;
`/go/` and click logging; conversions ingest; amazon.com excluded from network conversion; brand claim,
responses under moderation, corrections; batch two (toppers, pillows), three (sheets, comforters), four
(bases, kids, cribs) per the expansion rule.
Acceptance: first tracked commission; offer coverage on 90 percent of indexable models; brand claim flow
tested.

### Phase 6: Payments and premium (8 days, when the section 3 trigger is met)
Stripe Checkout, Billing, Portal, Tax; webhooks; trial and auto-renewal disclosures; one-click cancel;
reports; premium-only assets.
Acceptance: subscribe, upgrade, cancel, refund tested in test mode; reconciliation passes; cancel flow meets
state auto-renewal requirements.

### Phase 7: Scale and B2B (later)
Brand analytics and licensing with minimum cell sizes; public API; Instagram, X, YouTube automation with
audits; second category families.
Acceptance: per-feature, defined at planning time.

## 20. Scaling path

| Trigger | Change |
|---|---|
| Database over 4 GB or ranking queries over 500 ms | Compute upgrade; partition observations, price points, and Search Console tables by month |
| n8n executions over 50k per month or median step over 2 minutes | Queue mode with workers, or hot workflows to edge functions |
| Search latency over 300 ms | Typesense or Meilisearch; pgvector with a small embedding model |
| More than three AI providers | Gateway |
| International sales | Merchant of Record |
| Review volume over 1,000 per day | Dedicated moderation tier; human moderator contract |
| Categories beyond sleep | Same schema; new family, rubric, collectors |

## 21. Risks and mitigations

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Google treats the site as an aggregator or scaled content | Medium | Severe | Owned-signal requirement in the gate, native reviews, first-party price history, small evidence-driven batches, methodology and author |
| Amazon acts against collection | Medium | High | Managed provider, logged-out collection, no Associates account; owned-signal rule means losing Amazon cannot flip a page's indexability; de-index playbook; second sources |
| First-party statements draw a claim (summaries, brand hubs, deal pages) | Low to medium | Medium | Every public-record claim cites a source row; sale claims phrased as observed prices; correction channel; moderation records |
| Empty-page perception at launch | High if unmanaged | High | Gate requires data card, owned signal, and evidence |
| Fake, insider, or incentive-conditioned reviews | Medium | High | Tiers, material-connection question and disclosure, moderation, no sentiment conditioning, published dispute stats |
| Gated content leaks into cached pages | Medium | High | Visitor-only ISR, client-fetched gated fragments, CI leak test |
| Affiliate reversals and cash lag | High | Medium | Conservative model, diversified programs, code validation |
| Operator overload | Medium | High | Bulk queues, budget reservation, routine under four hours, kill switches |
| AI cost drift | Medium | Medium | Reserve-and-settle budgets, batch and caching, input hashes |
| Vendor or API terms change | Medium | Medium | Adapters isolated; source policies in data; Postgres owns the asset |
| Platform API access for social delayed | High | Low | Pinterest and email first; others deferred with lead times |

## 22. KPIs and targets

| KPI | Q1 after launch | Q2 | Q4 |
|---|---|---|---|
| Indexable URLs | 40 to 90 | 150 to 250 | 300 to 500 (evidence-driven) |
| Organic sessions per month | 1k to 4k | 4k to 12k | 10k to 30k |
| Native reviews (cumulative) | 200 | 800 | 3,000 |
| Visitor to review conversion | 0.3% | 0.5% | 0.8% |
| Email list (consented) | 300 | 1,500 | 6,000 |
| Tracked affiliate sales per month | 2 to 8 | 10 to 30 | 40 to 120 |
| Cost per indexable page (all-in) | under $8 | under $5 | under $3 |
| Operator hours per week | under 6 | under 4 | under 4 |

## 23. Open decisions and assumptions

Decided: niche, brand, platform model, public reviews with analytics unlock, seeding by collected reviews
plus a founding campaign, no Amazon Associates while collecting, n8n, Supabase, Next.js on Vercel, Stripe
later, review incentive terms, zero-spend build with about $500 of one-time Phase 0 costs.

Open: n8n during build (Docker locally or Railway); operator hours per week and target date; unlock duration
(12 months proposed); collection budget for month one ($50 proposed); whether counsel finalizes the legal
documents; Discord for alerts (later).

Assumptions to verify before Phase 1: Apify actor pricing and incremental support; affiliate program rates
and new-site acceptance; keyword volumes from a paid tool; Pinterest API approval lead time.

## Appendix A: document index

`CLAUDE.md` conventions. `docs/01` tooling audit and critique. `docs/02` decisions log. `docs/03` software
proposal v0.1 (superseded where this plan differs). `docs/04` evidence strategy. `docs/05` source plans.
`docs/06` niche and attribution. `docs/07` niche rankings v3. `docs/08` mattress comparison model. `docs/09`
sleep niche plan. `docs/10` SEO and growth plan. `docs/11` platform decision. `docs/12` seeding decision.
`docs/research/` evidence appendices.

## Appendix B: glossary

Observation: one paraphrased claim about one product on one criterion from one source or native review, with
a pointer. Native review: a review submitted on this site by a verified owner. Data card: specs, warranty,
certifications, price. Owned signal: first-party price history or native reviews. Index gate: the single
rule deciding indexability. Shrunk mean: an average pulled toward the category prior in proportion to how
few ratings exist. Unlock: 12 months of analytics access granted per published review. Batch: URLs released
to the index together and measured as a group.

## Appendix C: review log

Review 1 (architecture and feasibility, 35 findings) and Review 2 (legal, SEO, trust, revenue, 30 findings)
were applied in v1.1. Principal changes: one index-gate rule with an owned-signal requirement; visitor-only
ISR with client-fetched gated content and a leak test; `security_invoker` views and column grants; dirty-flag
category scoring with fully specified math; review status machine and `publish_review`; single entitlement
source; `pages` as the only indexability truth; current-row semantics on scoring tables; typed citations;
full-row change log with `revert_run`; natural-key idempotency; pg_trgm instead of embeddings at launch;
cheaper price tracking and incremental collection; microusd money; budget reservation; two photo buckets
and evidence deletion; material-connection question and tier renaming; enumerated disputes; moderated
brand responses; free badges with optional links; corrected structured data and AggregateRating; YouTube
30-day compliance; Walmart deferred; conservative unit economics and KPIs; privacy retention schedule;
social scope cut to Pinterest and email; sweepstakes rules; Vercel and Supabase paid-tier timing.

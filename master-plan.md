# RateMyBed Master Plan

Version 1.0 draft, 2026-09-29. Status: FOR OPERATOR REVIEW. This document is the single source of truth for
scope, architecture, and build order. Where it conflicts with an earlier document in `docs/`, this document
wins; where it is silent, the numbered documents in `docs/` apply. Research evidence lives in
`docs/research/`. Conventions that every build session must follow live in `CLAUDE.md`.

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
17. [Security and compliance](#17-security-and-compliance)
18. [Cost model and unit economics](#18-cost-model-and-unit-economics)
19. [Build plan](#19-build-plan)
20. [Scaling path](#20-scaling-path)
21. [Risks and mitigations](#21-risks-and-mitigations)
22. [KPIs and targets](#22-kpis-and-targets)
23. [Open decisions and assumptions](#23-open-decisions-and-assumptions)
- [Appendix A: document index](#appendix-a-document-index)
- [Appendix B: glossary](#appendix-b-glossary)

---

## 1. Executive summary

RateMyBed (ratemybed.com) is a review platform for beds and sleep products. Verified owners rate their beds
in a structured form; those ratings are public and indexable. On top of the ratings sits an intelligence
layer: sentiment summaries derived from collected third-party reviews, objective spec and warranty data,
price history, a versioned scoring rubric, comparisons, rankings, and a quiz that routes visitors to the right
products. Submitting one review unlocks the analytics layer; subscribing unlocks more. Revenue comes first
from affiliate commissions on top-rated products and comparisons, then from premium access, and later from
brand analytics and data licensing.

The system is designed to be run by one person. All state lives in Postgres (Supabase) with provenance on
every fact. n8n orchestrates collection, extraction, scoring, page generation, moderation, alerts, and
growth loops as thin stateless workflows. AI is used only where interpretation is required (extraction,
classification, summarization, QA, moderation triage) through a model registry with hard cost caps;
everything mathematical is SQL. The site is a Next.js application on Vercel that renders from the database
and includes the admin mission control. The build phase costs nothing; the first unavoidable costs are
Vercel Pro and Supabase Pro when affiliate links go live, about $50 to $70 per month plus metered usage.

Google's 2025 and 2026 updates removed sites built from templated affiliate pages and rewarded sites with
owned, first-hand data. RateMyBed is built to be the second kind: native reviews, native scoring, real
comparisons, and a small index grown in evidence-driven batches. The growth engine measures Search Console
daily, proposes new pages where demand appears, prunes pages that do not earn their place, and feeds email,
YouTube, Pinterest, and citation channels from the same database, so the asset compounds without the
operator writing pages.

## 2. Vision, principles, non-negotiables

Vision: the place people go to find out what a bed is actually like to own, from people who own it and from
every credible source, organized so the right bed for a specific person is obvious.

Principles (from `CLAUDE.md`, restated as engineering requirements):

1. Trust first. Every claim traces to a stored row with provenance. Every generated sentence cites
   observation or review IDs. Nothing is published that the data does not support.
2. Value in five things: native reviews, native scoring, comparisons, review summaries, suggestions.
3. One operator. Every component must be operable through dashboards and approval queues, not code
   changes. Weekly operator load target: under four hours.
4. Zero spend until unavoidable. Free tiers during build; paid tiers only when a limit or a commercial
   rule forces them; every AI and data call capped by budget.
5. Autonomy with oversight. Pipelines run themselves; publication, moderation edge cases, and growth
   proposals pass through queues the operator clears in bulk.
6. Data outlives tools. Postgres is the asset; site, orchestrator, AI provider, payment processor, and
   collectors are replaceable.
7. Small index, deep pages. Expand only on Search Console evidence.
8. Run as a review platform: verification, moderation, published policy, brand responses, disputes,
   disclosed incentives, no suppression, DMCA agent.

Non-negotiables: no third-party review text republished; no collected reviews in the native section or in
AggregateRating; no Reddit scraping; no Gemini grounding for stored data; no Amazon Associates account while
Amazon collection runs; no bought links; no paid placement in rankings; affiliate disclosure adjacent to
every affiliate link with rel="sponsored".

## 3. Business model and revenue streams

Ordered by activation. Each stream has a trigger condition so nothing is built before it can earn.

| # | Stream | Mechanism | Trigger to activate | Unit economics notes |
|---|---|---|---|---|
| 1 | Affiliate commissions (primary) | Flat bounties and percentages from brand programs (DreamCloud $150, Purple to $150, Nolah $105 to $160, Leesa $75, Saatva 3%, Helix 6 to 12%, Parachute ~15%, Cozy Earth to 25%, Eight Sleep to $180); retailer programs (Wayfair 7%, Mattress Firm 3 to 4%); sub-affiliate networks on day one (Sovrn, FlexOffers) | Skeleton site with policies live; applications approved | Commissions reverse on returns inside 100 to 365 night trials; plan for a 90 to 120 day cash lag and 20 percent reversal on mattresses |
| 2 | Brand-specific promo codes | Codes issued by brand programs shown on model and comparison pages | Same as 1 | A large share of mattress affiliate revenue flows through codes; track code use per page |
| 3 | Premium membership | Monthly and annual; unlocks full analytics, saved personalized rankings, unlimited comparisons, alerts, weekly digest; seven-day trial with card | Measurable organic traffic (target 10k sessions per month) | $6 to $9 per month, $49 to $69 per year, founding-member price; Stripe |
| 4 | One-time reports | Per-model or per-category buying report with price history and durability data | With premium | $4 to $12; Stripe Checkout |
| 5 | Verified brand accounts | Brands claim their pages, respond to reviews, see their own analytics; badge for earned ranks; no influence on rankings | 50 or more reviews for the brand's products | $49 to $199 per month per brand; must never touch scoring |
| 6 | Brand analytics and data licensing | Complaint frequency, return reasons, segment satisfaction, competitor benchmarks; anonymized survey and review data | 5,000 or more native reviews | Quarterly reports or API; B2B pricing |
| 7 | Deal and sale-event sponsorship | Sponsored placement in the deal digest and sale pages, clearly labeled, never in rankings | Email list of 10,000 or more | Flat fee per send |
| 8 | Lead-generation adjacent | Mattress removal and recycling services, sleep clinics, adjustable-bed installers, hot tub and sauna dealers (from the home-systems research) | Traffic in those page types | Per-lead payouts; TCPA compliance required for call leads |
| 9 | Display advertising | Mediavine or Raptive style network | 50k sessions per month, and only if pages stay fast | Low priority; degrades page quality |
| 10 | Public API and data feeds | Read-only API keys over published views for developers, retailers, and analysts | Growth stage | Usage-based |
| 11 | Embeddable tools and white-label quiz | Sheet-fit calculator, size chart, quiz for retailers and clinics with attribution or license | After tools exist | Links for free embeds; license fee for white-label |

Revenue principle: rankings and scores are never for sale. Anything a brand pays for is labeled and sits
outside the scoring path.

## 4. Product specification

### 4.1 Users and roles

| Role | Who | Can do |
|---|---|---|
| Visitor | Anyone | Read everything public: reviews, scores, summaries, rankings, comparisons up to three products, quiz with basic results |
| Reviewer | Visitor who submitted one verified review | Unlocked analytics for 12 months, renewable by a follow-up review; saved items; alerts on up to five products |
| Member (Premium) | Paying subscriber | Everything a Reviewer has plus saved personalized rankings, unlimited comparisons and alerts, full history, weekly digest, reports included with limits |
| Brand (verified) | Brand representative | Claim pages, respond to reviews, view own analytics; no effect on scores |
| Operator (admin) | Nicholas Moeller | Mission control, moderation, publishing, rubric versions, budgets, rollbacks |

### 4.2 Site map and page types

All routes render from the database. Page types and index policy:

| Route pattern | Page type | Indexable when |
|---|---|---|
| `/` | Home: quiz entry, top rated, recent reviews, sale calendar | Always |
| `/rate/` and `/rate/[model]` | Review submission flow | Never (noindex) |
| `/reviews/[brand]/[model]/` | Model page: data card, native reviews, sentiment summary, score panel, price history, alternatives, offers | Complete data card and (20+ observations across 2+ sources or 5+ native reviews) |
| `/brands/[brand]/` | Brand hub: lineup, warranty terms, price history, complaint profile, certifications, FTC history, fiberglass status | 3+ indexable models |
| `/compare/[a]-vs-[b]/` | Comparison page | Both models indexable and pair has demand signal (Search Console or curated top pairs) |
| `/best/[category]/` and `/best/[category]/[modifier]/` | Rankings | Category has 8+ indexable models; modifier only where rubric has evidence for it |
| `/quiz/[type]/` | Find-your-bed and variants | Landing indexable; results pages noindex |
| `/data/[study]/` | Data studies and tools (warranty index, discount honesty, fiberglass map, sag by year, size chart, sheet-fit) | Always once published |
| `/deals/` and `/deals/[event]/` | Price-history-backed sale pages | Always; regenerated per event |
| `/methodology/`, `/review-policy/`, `/about/`, `/disclosure/`, `/privacy/`, `/terms/` | Trust pages | Always |
| `/account/*` | Saved items, alerts, membership | Never |
| `/admin/*` | Mission control | Never; role-gated |
| `/go/[offer]` | Affiliate redirect with click logging | Never; 302 with rel="sponsored" on source links |

### 4.3 Review platform

Submission form fields (required unless noted):
- Product: brand, model, size, purchase month and year, price paid (optional), retailer, variant (firmness or
  type) where the model has variants.
- Sleeper context: position(s), body weight band (under 130, 130 to 230, over 230), partner (yes or no and
  partner band), primary need (pain, temperature, motion, edge, none).
- Ratings (1 to 5 each): comfort and support, durability so far, temperature, motion isolation, edge support,
  delivery and setup, customer service, value; overall.
- Text: what you like (min 40 characters), what you dislike (min 40 characters), free comment (optional).
- Evidence: photo of the bed or law label (required); receipt, order email, or serial (optional, raises
  verification tier).
- Consent and disclosure acknowledgements: content license, truthful-review attestation, incentive
  disclosure notice.

Verification tiers: Tier 1 photo only ("Owner"); Tier 2 photo plus purchase evidence ("Verified purchase").
Both display; Tier 2 carries more weight in aggregation (see 8.3).

Moderation: AI pre-screen for abuse, spam, off-topic, prohibited claims, and duplicate or near-duplicate
text; automatic publish when clean and verified; human queue for flags; appeal path by email; every
decision logged. No review is removed for being negative. Reviews from brand-affiliated accounts are
labeled, not hidden.

Follow-ups: at 6, 12, and 24 months the reviewer is asked "how is it holding up" with a short update form
(durability rating, sag observed, would still buy). Updates append to the review and feed the durability
dataset. A follow-up renews the analytics unlock.

Brand responses: verified brands may post one public response per review; responses are labeled and cannot
edit the review.

Disputes: reviewers and brands can flag a review; the operator resolves within the policy; outcomes logged.

### 4.4 Unlock and paywall tiers

| Capability | Visitor | Reviewer (unlocked) | Premium |
|---|---|---|---|
| Read all reviews, scores, summaries, rankings | Yes | Yes | Yes |
| Comparisons | Up to 3 products, not saved | Unlimited, saved | Unlimited, saved, shareable |
| Filters on reviews by body band, position, need | No | Yes | Yes |
| Durability by year, complaint frequency, sentiment by source | Headline numbers only | Full | Full |
| Price history | Last 30 days | 12 months | Full plus alerts |
| Personalized weighted rankings | Preview | Full, not saved | Full, saved |
| Quiz | Basic results | Full results with alternatives | Full plus saved profiles |
| Alerts | None | 5 products | Unlimited plus weekly digest |
| Reports | None | None | Included with limits |

The unlock lasts 12 months from the review date and renews on any follow-up review. Everything in the
Visitor column is indexable; nothing gated is needed for a page to make sense.

### 4.5 Quiz

Inputs: sleeper position, body band, partner, pain areas, temperature, firmness preference, motion
sensitivity, edge use, materials or allergies, adjustable base, size, budget, delivery preference, trial
importance. Each answer sets rubric weights or filters. Output: three picks with plain-language reasons
drawn from scores and observations, one "avoid if" line, offers with disclosure, and an email capture that
enrolls the visitor in price alerts for the picks. Variants: pillow, sheets, topper, kids' bed, crib
mattress checker. No AI at request time; the explanation text is templated from the score deltas. Aggregated
answers are stored as demand data.

### 4.6 Comparisons and rankings

Comparison page: side-by-side data card, score panel with per-criterion bars, warranty terms parsed, price
history overlay, native review counts and averages by sleeper band, sentiment summary deltas, "choose A if,
choose B if" generated from the deltas, offers. Rankings: filtered views over the same scores with the
rationale written from the rubric; "top rated" uses shrunk owner ratings blended with objective scores; ties
broken by evidence volume; every list shows evidence counts and last-verified dates.

### 4.7 Analytics layer (the unlock)

Per model: rating distribution by criterion; satisfaction by body band and position; durability by
ownership year; complaint frequency table; sentiment by source type (native, retailer, video, editorial);
price history with sale events; warranty strength; return experience summary. Per category: brand rollups,
discount honesty index, warranty index, fiberglass map. All computed by SQL views refreshed on data change.

### 4.8 Alerts and email

Transactional (Resend): verification, review published, follow-up requests, alert triggers. Marketing
(Klaviyo): weekly deals digest, sale-event previews five days before each of the five mattress sale
holidays, quiz follow-up with alternatives, founding-reviewer campaign. Alert types: price below target,
price drop percentage, real-sale detected, rank change, new model above score, follow-up due.

### 4.9 Brand portal

Claim flow with domain-email verification; response composer; analytics for own products; badge assets for
earned ranks with required link-back; billing through Stripe when the paid tier activates. No access to
scoring inputs or to other brands' raw data.

### 4.10 Admin mission control

Today panel (runs, failures, drafts, spend vs budget, reviews pending, verifications pending); pipeline
runs and dead-letter queue; draft and publish queue with bulk actions and evidence panel; moderation and
verification queues; entity merge and split queue; change log with revert; sources and quotas; affiliate
offers and link health; cost ledger by task and model; growth panel (Search Console by page type, proposed
pages, losers); membership metrics when payments exist; rubric versions and rescoring; budgets and kill
switches.

## 5. System architecture

```
   Sources (managed collectors, APIs, site forms)
        |
        v
  +-------------+     +---------------------+     +----------------------+
  | n8n         | --> | raw_content (TTL)   | --> | observations         |
  | collectors  |     | source_documents    |     | (paraphrased, cited) |
  +-------------+     +---------------------+     +----------+-----------+
                                                             |
  +-------------+     +---------------------+                |
  | site review | --> | reviews, updates,   | ---------------+
  | forms (Next)|     | verifications       |                |
  +-------------+     +---------------------+                v
                                              +--------------------------+
                                              | SQL: aggregation, rubric |
                                              | scoring, rankings, gates |
                                              +-----------+--------------+
                                                          |
                    +-------------------------------------v-----------------------------+
                    | Supabase Postgres: catalog, evidence, reviews, scoring, content,   |
                    | commerce, accounts, ops. pg_cron schedules, pgmq queues, RLS,     |
                    | change_log triggers, published views                              |
                    +-----------+--------------------------------------+----------------+
                                |                                      |
                    +-----------v-----------+              +-----------v-----------+
                    | Next.js public site   |              | Next.js /admin        |
                    | ISR from views        |              | role-gated            |
                    +-----------------------+              +-----------------------+
                                |
                    +-----------v-----------+   +---------------+   +----------------+
                    | Resend / Klaviyo      |   | Stripe (later)|   | Social posting |
                    +-----------------------+   +---------------+   +----------------+
```

Rules: all state in Postgres; n8n workflows are stateless step executors triggered by cron or queue
messages; deterministic logic is SQL or small edge functions with tests; AI calls go through one
sub-workflow that reads the model registry and the budget; the site reads published views only; every
write to tracked tables is logged by trigger and reversible.

## 6. Technology stack and tools

| Layer | Choice | Type | Cost now | Cost when live | Why | Replacement if outgrown |
|---|---|---|---|---|---|---|
| Database, auth, storage, cron, queues, vectors | Supabase | Low-code | Free (500 MB, pauses after 7 idle days) | Pro $25/mo | Plain Postgres, table editor plus SQL, MCP for agents, RLS | Managed Postgres with the same schema |
| Orchestration | n8n self-hosted (Docker locally during build; Railway or small VPS in production) | No-code | $0 | $5 to $12/mo | Visual, native Postgres and AI nodes, error workflows, Sustainable Use License covers internal use | Supabase-native pg_cron plus edge functions for hot paths; n8n queue mode |
| Site and admin | Next.js on Vercel, AI-maintained | Code (operator never edits) | Hobby free (non-commercial) | Pro $20/mo when affiliate links go live | ISR, structured data, admin in one deploy | Same framework elsewhere |
| AI | Anthropic direct: Haiku 4.5 for extraction, classification, moderation triage (Batch API, caching); Sonnet 5.5 for summaries, comparison narration, QA; OpenAI small model for independent QA when a key exists | Config | Metered, capped | $20 to $150/mo by volume | No gateway fee; model registry makes swaps a row change | OpenRouter or Vercel AI Gateway if providers exceed three |
| Collection | Apify actors (Amazon and retailer reviews), Firecrawl (manufacturer and editorial pages), YouTube Data API, Walmart Affiliate API, Best Buy API, Wayfair feed, Serper or Brave for discovery | Low-code | Free credits | $0 to $60/mo | Managed, per-run, no scraper maintenance | Second provider |
| Email | Resend (transactional), Klaviyo (marketing, already connected) | No-code | Free tiers | Resend $20 at 50k | Good APIs, n8n reachable | Postmark, SES |
| Payments (later) | Stripe Checkout, Billing, Customer Portal, Tax | No-code checkout | $0 | 2.9% + 30c, +0.7% Billing, +0.5% Tax | Instant onboarding, best tooling; US-only launch | Polar or Paddle for Merchant of Record if international |
| Monitoring | Sentry (free), Better Stack (free), n8n error workflows, cost ledger in Postgres | No-code | $0 | $0 to $26 | Enough for one operator | Paid tiers |
| Search | Postgres full-text plus pgvector | Built-in | $0 | $0 | Adequate to tens of thousands of products | Typesense or Meilisearch |
| Social posting | n8n to platform APIs (YouTube Data API uploads, Pinterest API, X API basic, Instagram Graph API via a Business account); Buffer or Publer as a fallback scheduler | Low-code | Free tiers | $0 to $30 | Autopilot from the database | Dedicated tools |
| Analytics | Google Search Console API, Plausible or Vercel Analytics, affiliate network APIs, all into Supabase | Low-code | $0 | $0 to $9 | One dashboard | Same |
| Repo and CI | GitHub, GitHub Actions for migrations and tests | Code | $0 | $0 | Migrations in git applied by CI | Same |

Development-time tooling (from the Step 0 audit): Supabase plugin and MCP against a dev project; the
custom `product-intel-conventions` skill created from this plan; Stripe plugin in Phase 6; Sentry and
Playwright at the end of Phase 3. No community-tier plugins.

## 7. Data model

Postgres schemas: `catalog`, `evidence`, `reviews`, `scoring`, `content`, `commerce`, `accounts`, `ops`.
Every table has `id uuid primary key`, `created_at`, `updated_at`. Tracked tables carry change-log triggers.

### 7.1 catalog
- `categories` (family, slug, name, status, config jsonb: max_products, max_sources_per_product,
  daily_ai_budget_usd, refresh_interval_days, min_observations, min_native_reviews)
- `brands` (name, slug, website, parent_company, affiliate_program_id, ftc_history jsonb, fiberglass_status)
- `products` (category_id, brand_id, canonical_name, slug, lifecycle_status, published_at,
  completeness_score, indexable bool, msrp_cents, image_url, summary_block_id)
- `product_variants` (product_id, name, firmness, size, model_number, upc, gtin, sku_map jsonb)
- `product_identifiers` (variant_id, type, value, source_document_id)
- `product_aliases` (product_id, alias, source_document_id)
- `entity_candidates` (raw_name, raw_model, source_document_id, proposed_product_id, confidence, decision,
  decided_by, decided_at)
- `attributes` (category_id, key, name, unit, datatype, is_objective, normalization jsonb)
- `spec_values` (product_id, variant_id nullable, attribute_id, value_text, value_num, unit, normalized_num,
  source_document_id, confidence, collected_at, verified_at, run_id, is_canonical)
- `warranty_terms` (product_id, years, prorated_after_years, sag_threshold_inches, exclusions jsonb,
  trial_nights, return_fee_cents, return_conditions, source_document_id, verified_at)
- `certifications` (product_id or brand_id, scheme, certificate_id, verified_at, source_url)

### 7.2 evidence
- `sources` (key, name, type, license_class, retention_days, allow_excerpt bool, excerpt_max_words,
  rate_limit, cost_per_call_cents, credentials_ref)
- `source_documents` (source_id, product_id nullable, url, title, fetched_at, content_hash, expires_at,
  robots_ok, provider_run_id)
- `raw_content` (source_document_id, body, mime, expires_at) with a nightly purge job
- `observations` (product_id, criterion_key nullable, source_document_id, kind, polarity, strength 1 to 3,
  excerpt varchar(200), pointer jsonb, sleeper_context jsonb, extracted_by_model, prompt_version, run_id,
  superseded_by)
- `price_points` (offer_id, price_cents, list_price_cents, observed_at, on_sale bool, event_tag)

### 7.3 reviews
- `reviews` (product_id, variant_id, user_id, purchase_month, price_paid_cents, retailer, sleeper jsonb,
  ratings jsonb, overall smallint, likes text, dislikes text, comment text, status, verification_tier,
  incentive_disclosed bool, brand_affiliated bool, published_at, ip_hash, device_hash)
- `review_updates` (review_id, at_months, durability, sag_observed bool, still_recommend bool, comment,
  created_at)
- `review_evidence` (review_id, kind photo|receipt|serial, storage_path, verified bool, verified_by,
  verified_at)
- `moderation_events` (review_id, actor_type, action, reason, model, created_at)
- `brand_responses` (review_id, brand_user_id, body, published_at)
- `disputes` (review_id, raised_by, reason, status, resolution, resolved_at)
- `consents` (user_id, kind, version, accepted_at)
- `survey_responses` (survey_id, respondent_hash, product_id, answers jsonb, publishable bool, collected_at,
  panel_provider) for the optional later panel program

### 7.4 scoring
- `rubrics` (category_id, version, status, weights jsonb, notes)
- `rubric_criteria` (rubric_id, key, name, type objective|subjective|owner, weight, normalization jsonb,
  evidence_floor int, prior_mean numeric, prior_weight int)
- `assessments` (product_id, rubric_id, criterion_key, score, rationale, evidence_ids uuid[], evidence_count,
  confidence, model, prompt_version, run_id)
- `owner_aggregates` (product_id, criterion_key, n, mean, shrunk_mean, n_tier2, by_band jsonb, by_position
  jsonb, computed_at)
- `scores` (product_id, rubric_id, criterion_key nullable, kind, value, confidence, computed_at, run_id)
- `rankings` (category_id, rubric_id, modifier_key nullable, product_id, rank, score, evidence_count,
  computed_at)

### 7.5 content
- `content_blocks` (product_id or page_key, block_type, version, status, body jsonb, citations uuid[],
  generated_by, prompt_version, qa_report jsonb, approved_by, published_at)
- `pages` (route, page_type, product_id or category_id or pair, status, indexable bool, gate_report jsonb,
  last_built_at, last_verified_at)
- `comparison_pairs` (product_a, product_b, demand_signal, status)
- `data_studies` (slug, title, query_ref, dataset_path, published_at)

### 7.6 commerce
- `merchants` (name, network, program_id, cookie_days, rate_note, status)
- `affiliate_offers` (product_id, variant_id, merchant_id, url, promo_code, price_cents, currency,
  availability, price_as_of, last_verified_at, active)
- `clicks` (offer_id, page_route, user_id nullable, session_hash, at)
- `conversions` (network, offer_id, amount_cents, commission_cents, status, occurred_at, reversed_at)

### 7.7 accounts
- `profiles` (user_id, display_name, role, preferences jsonb, unlocked_until, brand_id nullable)
- `saved_items`, `saved_comparisons`, `user_weightings` (user_id, category_id, rubric_id, weights jsonb)
- `alerts` (user_id, type, target_id, threshold, active), `alert_events`
- `quiz_sessions` (user_id nullable, answers jsonb, results jsonb, email_captured bool)
- `plans`, `subscriptions`, `entitlements` (user_id, key, value, source review|plan|manual, expires_at),
  `payment_customers`, `payment_events`, `usage`

### 7.8 ops
- `pipeline_runs`, `pipeline_steps` (input_hash, output_hash), `ai_calls` (task, provider, model,
  input_tokens, output_tokens, cached_tokens, cost_cents, latency_ms, prompt_version),
  `budgets` (scope, period, limit_cents, spent_cents, kill_switch bool), `change_log` (table, row_id, field,
  old_value, new_value, actor_type, actor_id, reason, run_id, reverted_by), `models` (task, provider,
  model_id, max_cost_per_call_cents, batch_allowed, active), `prompt_versions`, `search_console_daily`
  (route, query, impressions, clicks, position, date), `growth_proposals` (kind, payload, status),
  `settings` (key, value)

Indexes on all foreign keys; composite indexes on `observations(product_id, criterion_key)`,
`spec_values(product_id, attribute_id)`, `price_points(offer_id, observed_at)`, `reviews(product_id,
status)`, `search_console_daily(route, date)`. pgvector columns on `observations.excerpt` embeddings and
`content_blocks` for semantic search and duplicate detection. RLS on all `accounts` and `reviews` tables.

## 8. Backend design

### 8.1 Layout
Supabase project per environment (dev, prod). Migrations in `supabase/migrations/` applied by GitHub
Actions. Published data exposed through views in a `public_api` schema that the site and any future API
read; write paths are RPC functions and edge functions only.

### 8.2 SQL functions (deterministic core)
- `normalize_spec(attribute_id, value)`: applies the attribute's normalization (min-max within category,
  log, thresholds, direction) and returns 0 to 10.
- `aggregate_owner_ratings(product_id)`: per criterion, n, mean, and shrunk mean using
  `shrunk = (n / (n + m)) * mean + (m / (n + m)) * prior`, with `m = prior_weight` (default 10) and `prior`
  the category mean; Tier 2 reviews weight 1.0, Tier 1 weight 0.7; brand-affiliated reviews weight 0.
- `compute_scores(product_id, rubric_id)`: objective criteria from normalized specs; owner criteria from
  shrunk means; subjective criteria from assessments when evidence_count is at or above the floor, else
  unscored; overall = weighted sum over scored criteria renormalized to available weight, with a coverage
  penalty of up to 15 percent when coverage is below 70 percent; writes `scores` and confidence.
- `compute_rankings(category_id, rubric_id, modifier_key)`: orders by overall, breaks ties by evidence
  count, writes `rankings`.
- `personal_ranking(user_id, category_id, weights)`: same math with user weights, computed at request time
  from `scores`; no AI.
- `index_gate(product_id)`: returns pass or fail with reasons per the rule in `CLAUDE.md`, plus
  unique-content ratio from block lengths; sets `pages.indexable`.
- `effective_entitlements(user_id)`: merges active subscription plan, review unlock (`unlocked_until`),
  manual grants; returns key-value set.
- `transition_product(product_id, to_status, actor, reason)`: enforces the lifecycle state machine and logs.
- `detect_price_events()`: nightly; computes real-drop percentage against the 90-day median, flags sale
  events, matches alerts, writes `alert_events`.
- `budget_check(scope)`: returns remaining budget; used before every metered call; flips kill switch.

### 8.3 Scheduled jobs (pg_cron)
Nightly: purge expired raw content; price event detection; entitlement expiry; Search Console pull enqueue.
Weekly: refresh enqueue for products past `next_refresh_at`; link health; growth proposals; digest build.
Monthly: rubric drift report; cost report.

### 8.4 Queues (pgmq)
`collect`, `extract`, `assess`, `score`, `build_page`, `qa`, `publish`, `moderate`, `verify`, `notify`,
`social`. Each message carries the entity id, run id, and attempt count; three retries with backoff for
transport errors, none for validation failures; dead-letter queue surfaced in admin.

### 8.5 Edge functions
`submit_review` (validates, stores, uploads evidence, enqueues moderate and verify); `stripe_webhook`
(signature check, idempotent by event id); `affiliate_redirect` (logs click, 302); `search_console_sync`;
`quiz_result` (runs `personal_ranking`, stores session); `brand_claim`.

### 8.6 Storage
Buckets: `review-evidence` (private, signed URLs for moderation; photos re-encoded and stripped of EXIF
before any public display), `product-images` (public), `datasets` (public CSV exports for data studies).

### 8.7 Search
Postgres full-text over products, brands, and content blocks; pgvector for near-duplicate review detection
and semantic product search. Upgrade trigger: query latency over 300 ms on search pages.

### 8.8 Security
RLS on every user-facing table; service role only inside n8n and edge functions; secrets in Supabase Vault,
n8n credentials, and Vercel environment; signed webhooks; rate limits on review submission and quiz;
Cloudflare or Vercel WAF rules when abuse appears. See section 17.

## 9. Workflow catalog

Every workflow: trigger, inputs, steps, outputs, failure handling, budget scope. All write `pipeline_runs`
and `pipeline_steps`; all call `budget_check` before metered steps.

| ID | Workflow | Trigger | Steps | Outputs | Cost scope |
|---|---|---|---|---|---|
| W01 | Catalog ingest | Weekly cron; manual | Fetch brand lineup and product pages (Firecrawl), extract specs and warranty terms (AI extraction), resolve entities, write spec_values and warranty_terms with provenance | Products, variants, specs, warranties | collection, ai_extract |
| W02 | Price tracker | Daily cron | Fetch prices per offer from brand pages and retailer APIs; write price_points; run detect_price_events | Price history, sale events | collection |
| W03 | Third-party review collection | Queue `collect`; weekly per product | Run managed actor for the product's review sources (Walmart API, YouTube, Apify actors for Amazon and retailer pages, Firecrawl for editorial); write source_documents and raw_content with TTL | Raw content | collection |
| W04 | Observation extraction | Queue `extract` | Batch raw content to Haiku with the extraction schema; validate; write observations with excerpts of 25 words or fewer and pointers; mark superseded | Observations | ai_extract |
| W05 | Sentiment summary | Queue `assess` when observation count changes by 10 percent or more | Build criterion summaries from observations only (Sonnet), cite observation ids, QA pass; write content_blocks | Summary blocks | ai_write |
| W06 | Review moderation | Queue `moderate` on submission | Haiku classifier for abuse, spam, duplicates, prohibited claims; auto-publish when clean; else admin queue | Review status | ai_moderate |
| W07 | Verification | Queue `verify` | Photo checks (image present, not stock, EXIF stripped), receipt parsing when supplied; set tier | Verification tier | ai_extract |
| W08 | Scoring and ranking | Any change in specs, observations, reviews, or rubric | Run aggregate_owner_ratings, compute_scores, compute_rankings; log deltas | Scores, rankings | none |
| W09 | Page build | Queue `build_page` | Assemble blocks; generate comparison narration and "choose A if" from deltas; run index_gate; write pages and drafts | Page rows, drafts | ai_write |
| W10 | QA | Queue `qa` | Independent model checks each claim against cited rows; flags unsupported claims; scores draft | qa_report | ai_qa |
| W11 | Publish | Admin approval or auto-publish per category setting | Set published, trigger ISR revalidation, regenerate sitemaps, recompute internal links, IndexNow ping | Live pages | none |
| W12 | Refresh | Weekly cron | Hash manufacturer pages, check new reviews and videos since last run, re-run only changed steps, detect successor models | Updated data | collection, ai_extract |
| W13 | Alerts and email | Nightly after detect_price_events; event-driven | Batch alert_events by user, send via Resend; weekly digest via Klaviyo | Emails | email |
| W14 | Review follow-ups | Daily cron | Find reviews at 6, 12, 24 months; send update requests; renew unlock on completion | Updates | email |
| W15 | Search Console ingest | Daily cron | Pull impressions, clicks, position by route and query into search_console_daily | Metrics | none |
| W16 | Growth proposals | Weekly cron | Propose comparison pairs with impressions but no page; modifier pages with query evidence; noindex pages with zero impressions after 8 weeks and thin data; flag models with clicks but no price data | growth_proposals | none |
| W17 | Social autopilot | Daily cron | Select data events (price drop, new top-rated, study published); render post text and image card; post to Pinterest, X, Instagram, YouTube Shorts (video assembled from template); log | Posts | ai_write, social |
| W18 | Affiliate link health | Weekly cron | HEAD each offer URL, check redirect target and availability; deactivate broken offers; admin report | Offer status | none |
| W19 | Budget guard | Every 15 minutes | Sum ai_calls and provider spend per scope; pause queues when a limit is hit; alert | Kill switches | none |
| W20 | Daily health report | Daily cron | Runs, failures, drafts, moderation backlog, spend, expiring documents, growth summary; email (Discord later) | Report | email |
| W21 | Entity resolution | Queue after W01 and W03 | Blocking on brand plus model tokens; identifier match; AI adjudication for ambiguous; queue merge/split for low confidence | entity_candidates decisions | ai_extract |
| W22 | Data studies | Monthly cron | Run study queries, render charts and CSV, write data_studies and pages; enqueue social and journalist-pitch drafts | Studies | ai_write |

## 10. AI task registry

| Task | Model (initial) | Mode | Input | Output schema | Cap per call | Eval set |
|---|---|---|---|---|---|---|
| spec_extraction | Haiku 4.5 | Batch, cached system prompt | Page text | attribute key, value, unit, confidence | $0.02 | 20 pages |
| warranty_parse | Haiku 4.5 | Batch | Warranty text | years, prorated_after, sag_threshold, fees, exclusions | $0.02 | 15 documents |
| observation_extract | Haiku 4.5 | Batch | Review or comment text | criterion, polarity, strength, sleeper_context, excerpt (25 words max) | $0.01 | 50 reviews |
| entity_adjudicate | Haiku 4.5 | Online | Candidate pair with identifiers | same_product bool, variant_of bool, confidence | $0.01 | 30 pairs |
| review_moderate | Haiku 4.5 | Online | Review text plus metadata | flags[], severity, action | $0.005 | 40 reviews |
| sentiment_summary | Sonnet 5.5 | Online | Observations for product and criterion | summary text with citations, counts | $0.06 | 15 products |
| comparison_narrate | Sonnet 5.5 | Online | Score and spec deltas | "choose A if / choose B if" bullets with citations | $0.04 | 15 pairs |
| ranking_rationale | Sonnet 5.5 | Online | Rubric and top entries | rationale paragraphs with citations | $0.05 | 10 lists |
| qa_review | OpenAI small model (or Haiku until a key exists) | Online | Draft plus cited rows | unsupported_claims[], score | $0.03 | 20 drafts |
| social_copy | Haiku 4.5 | Online | Data event | platform-specific text variants | $0.005 | 20 events |
| journalist_pitch | Sonnet 5.5 | Online | Study and request | draft reply with citations | $0.03 | 10 |

Rules: structured outputs validated against schema; invalid output retried once then failed; prompts
versioned in `/prompts` and mirrored to `prompt_versions`; every call logged to `ai_calls`; per-scope
daily budgets (`ai_extract`, `ai_write`, `ai_qa`, `ai_moderate`, `collection`, `email`, `social`) with kill
switches; Batch API and prompt caching for anything not latency-sensitive; evals run before any prompt or
model change. Expected AI spend at launch scale (40 models, weekly refresh): under $30 per month.

## 11. Frontend design

### 11.1 Stack and rendering
Next.js App Router, TypeScript, Tailwind, server components reading `public_api` views through the
Supabase client with the anon key; ISR with on-demand revalidation from W11; no client-side fetching for
indexable content; images through Next Image with a Supabase storage loader; Plausible or Vercel Analytics.

### 11.2 Route structure
`app/(site)/…` for public routes in section 4.2; `app/(account)/…` behind auth; `app/admin/…` behind role
check; `app/api/…` only for the affiliate redirect and webhooks that are not edge functions.

### 11.3 Core components
DataCard (specs, warranty, certifications with source links); ScorePanel (overall, per-criterion bars,
confidence, evidence counts, rubric version link); ReviewList (filters gated, each review with tier badge,
sleeper context, incentive disclosure, brand response); SentimentSummary (per criterion, counts by source
type, "Sources" row collapsed but visible); PriceHistory (chart with sale events; 30 days for visitors);
OfferBlock (offers with price-as-of, disclosure line, rel="sponsored" links through `/go/`); ComparisonTable;
QuizFlow; UnlockPrompt (contextual, shows what a review unlocks); AlertButton; Methodology and Author blocks
on every ranking and comparison.

### 11.4 Structured data
Product with Review (author: RateMyBed editorial, reviewRating from our score) and AggregateRating from
native reviews only when n is 5 or more; positiveNotes and negativeNotes from the summary; BreadcrumbList;
Organization and Person; ItemList on rankings. Generated server-side from the same views.

### 11.5 Gating UX
Gated elements render a skeleton with real headline numbers and a one-line unlock prompt. No interstitials,
no hidden text tricks. Unlock state comes from `effective_entitlements` in a server component; the client
never decides access.

### 11.6 Performance and accessibility
Static or ISR for every indexable route; Core Web Vitals budgets enforced in CI with Lighthouse; semantic
HTML; keyboard-accessible forms; alt text generated from data for product images.

### 11.7 Admin
Same app, `app/admin`, server actions calling RPCs; tables with bulk actions; evidence side panel; diff view
for change log; charts from `search_console_daily` and `ai_calls`.

## 12. Entitlements and payments

Tiers per section 4.4. Entitlement resolution is a SQL function; sources are `review` (unlocked_until),
`plan` (active subscription), `manual` (comps, founders). The site never decides access client-side.

Payments (Phase 6): Stripe Checkout for subscription and one-time reports, Billing for trials and proration,
Customer Portal for self-service, Stripe Tax for US sales tax. Webhooks are idempotent by event id and update
`subscriptions`; a nightly reconciliation compares Stripe state with local state. Switching processors is a
webhook remap; entitlement logic is untouched. Merchant of Record (Polar or Paddle) only if international
sales begin.

## 13. Data acquisition and legal posture

| Source | Access | Store | Retention | Legal basis and rule |
|---|---|---|---|---|
| Manufacturer pages, spec sheets, warranty PDFs | Firecrawl, robots-respecting | Facts, parsed terms, links | Indefinite | Facts not copyrightable; no marketing text or images stored |
| Brand and retailer prices | Firecrawl, retailer APIs | Price points | Indefinite | Observed facts |
| Native reviews | Site form | Full | Indefinite, user deletable | User license in Terms; incentive disclosed; FTC rule compliance |
| Collected third-party reviews (Amazon and retailer pages via Apify; Walmart API; YouTube API) | Managed providers, APIs | Observations, 25-word excerpts, pointers; raw purged in 30 days | Raw 30 days; observations indefinite | Analysis and commentary; never republished; never in native section or AggregateRating; Amazon Associates excluded while Amazon collection runs |
| Editorial reviews | Discovery then direct fetch | Score, one-line quote, link | Indefinite | Fair-use citation |
| Certification directories (CertiPUR-US, OEKO-TEX, GOTS, GOLS, GREENGUARD, RDS, Supima) | Lookup | Certificate facts | Indefinite | Public directories |
| CPSC incidents and recalls; 16 CFR 1241 crib standard; state fiberglass law | API and public records | Facts | Indefinite | Public domain |
| Reddit | Search discovery | Thread URL and title only | Indefinite | Linking only |
| Sparse scoped web research (OpenAI or Claude web search) | API | Our paraphrase plus URLs | Indefinite | Output owned; citations displayed |

Platform compliance: Terms of Service with content license; Privacy Policy; Review Policy; Affiliate
Disclosure; DMCA agent registered; Consumer Review Fairness Act respected (no gag clauses); FTC Endorsement
Guides (disclosure adjacent to links and on incentivized reviews); no sentiment-conditioned incentives;
brand response and dispute processes; reviewer data deletable on request.

## 14. SEO and growth engine

Summarized from `docs/10-seo-and-growth-plan.md` and adjusted for the review-platform model.

1. Page-type order of attack: model review pages and brand hubs (they capture "[brand] reviews" and
   "[model] review" queries where user platforms outrank editorial sites), then comparisons, then modifier
   rankings, then data studies and tools, head terms last.
2. Index gate per `CLAUDE.md`; batch one of about 90 URLs; expansion only when 70 percent of a batch is
   indexed and impressions rise four weeks running; automatic noindex for zero-impression thin pages after
   eight weeks; 50 to 150 new indexable URLs per month in year one.
3. On-page automation: structured data, internal links computed on publish, templated titles with a unique
   headline number, visible last-verified dates, methodology and author links, disclosures.
4. Links and citations: monthly data studies with CSV; journalist-request workflow; brand badges with
   link-back; embeddable tools; explicit data-use permission.
5. Measurement loop: W15 and W16 feed the admin growth panel; operator approves proposals weekly.
6. AI answer engines: quantified claims, summary blocks that quote well, complete structured data,
   freshness; cited pages earn two to five times the clicks of uncited ones.

## 15. Social autopilot

Goal: every platform posts from the database on a schedule with no manual work, drives traffic to the site,
and builds the brand as the place where real owners rate beds.

| Platform | Content generated from data | Cadence | Mechanism |
|---|---|---|---|
| Pinterest | Product cards with score and top praise; size charts; sheet guides; sale calendars | 3 to 5 pins per day | Pinterest API via n8n; image cards rendered from a template (Satori or a headless browser in n8n) |
| YouTube Shorts and long-form | Price history of a model; warranty comparison of five brands; "is this sale real"; monthly data study | 2 shorts per week, 1 long per month | Script from data (AI copy), voiceover (TTS), template video (ffmpeg or a render API), upload via YouTube Data API |
| Instagram (Business) | Same cards as Pinterest; review quotes with consent; study charts | 1 per day | Instagram Graph API |
| X | Data events: real price drops, new top-rated, study threads | 1 to 3 per day | X API basic tier when justified; otherwise scheduler |
| TikTok | Repurposed shorts | 3 per week | Manual approval queue initially; API posting when the account qualifies |
| Email | Deals digest, sale-event previews, quiz follow-ups, review follow-ups | Weekly plus events | Klaviyo and Resend |
| Reddit and forums | None automated. Operator answers questions with data links within community rules | Ad hoc | Manual |

Rules: every post links to a database page; posts about reviews quote only native reviews with consent;
affiliate disclosure in captions where links are affiliate; a daily cap per platform; approval queue for
the first 30 days, then auto-post with a kill switch.

## 16. Operations and observability

- Mission control (section 4.10) is the operator's only daily surface.
- Alerts: budget breaches, dead-letter growth, moderation backlog over 24 hours, verification backlog,
  failed sends, uptime, Search Console drops over 30 percent week over week. Delivered by email now,
  Discord later.
- Weekly operator routine (target under four hours): clear draft queue in bulk; clear moderation flags;
  review growth proposals; approve journalist replies; glance at costs and losers.
- Runbooks in `docs/runbooks/`: pause all pipelines; roll back a run; re-run a product; rotate a credential;
  restore from backup; handle a brand dispute; handle a DMCA notice; handle a data-deletion request.
- Backups: Supabase Pro daily backups; weekly `pg_dump` to storage; migrations in git.
- Change history: trigger-based `change_log` with one-click revert per row and per run.

## 17. Security and compliance

Secrets only in Supabase Vault, n8n credentials, and Vercel environment; least-privilege API keys with
vendor spend caps; RLS everywhere; service role only server-side; webhook signatures; rate limits on forms
and the quiz; EXIF stripping and re-encoding of uploaded images; signed URLs for evidence; no PII in logs;
IP and device hashes only; data deletion flow; `/security-review` before every merge; dependency updates
monthly; Sentry for errors; WAF rules on abuse; COPPA not applicable (18+ reviewers stated in Terms).

## 18. Cost model and unit economics

| Stage | Monthly fixed | Metered | Notes |
|---|---|---|---|
| Build (months 0 to 2) | $0 | AI under $20; collection within free credits | Vercel Hobby, Supabase Free with keep-alive, n8n in Docker |
| Launch (affiliate links live) | Vercel Pro $20, Supabase Pro $25, n8n host $5 to $12 | AI $20 to $60, collection $0 to $60, email $0 | About $50 to $180 |
| Growth (500 models, 10k reviews) | Same plus Supabase compute $10 to $60 | AI $100 to $300, collection $60 to $200, email $20 to $60, social tools $0 to $30 | About $250 to $700 |
| Payments live | Stripe fees only | | 2.9% + 30c, +0.7%, +0.5% |

Unit economics at launch scale: 100 tracked mattress sales per month at a blended $90 commission with 20
percent reversal is about $7,200 gross per month against under $200 of costs. Reaching 100 sales requires
roughly 13,000 sessions per month at 25 percent click-out and 3 percent conversion, which the growth
targets place in the third quarter after launch. Premium at 300 members and $7 per month adds about
$2,100 per month.

## 19. Build plan

Effort is in operator-days assuming AI-assisted development with the operator approving rather than coding.

### Phase 0: Foundation (5 days)
Tasks: register ratemybed.com and .ai; claim handles; file trademark; create Supabase dev project; enable the
Supabase plugin; repo structure (`supabase/`, `web/`, `n8n/`, `prompts/`, `docs/runbooks/`); full schema
migration from section 7; seed categories, attributes, rubric v1.0 for mattresses; n8n in Docker with
Postgres credentials; Anthropic key with a $20 cap; create the `product-intel-conventions` skill from this
plan; draft Terms, Privacy, Review Policy, Disclosure for operator review; DMCA agent registration.
Acceptance: migrations apply clean in CI; `index_gate`, `aggregate_owner_ratings`, `compute_scores` pass
unit tests on fixture data; conventions skill loaded in a fresh session.

### Phase 1: Catalog and intelligence pipeline (15 days)
Tasks: W01 catalog ingest for the top 40 mattress models and 12 brands; W21 entity resolution with merge
queue; W02 price tracker; W03 collection for Walmart, YouTube, Apify actors, editorial; W04 extraction;
W05 sentiment summaries; W08 scoring; W10 QA; W19 budget guard; W20 health report.
Acceptance: 40 models with complete data cards; each with at least 20 observations from 2 sources; scores
and rankings reproducible; cost per model under $1.50; zero unsupported claims in QA on a sample of 10.

### Phase 2: Site and admin (15 days)
Tasks: Next.js app with routes in section 4.2; components in 11.3; structured data; admin mission control
with draft, moderation, entity, change-log, cost, and growth panels; W09 page build; W11 publish; sitemaps;
IndexNow; methodology, review policy, author, disclosure pages; Sentry; uptime.
Acceptance: batch one (about 90 URLs) built and approved through the admin; Lighthouse over 90 on model,
brand, and comparison pages; structured data validates; noindex on everything gated or incomplete.

### Phase 3: Review platform (12 days)
Tasks: Supabase Auth (magic link, Google); review submission flow with evidence upload; W06 moderation;
W07 verification; unlock entitlements; brand responses and disputes; follow-up flows W14; founding-reviewer
campaign assets; Playwright end-to-end tests for submit, moderate, publish, unlock.
Acceptance: a review goes from submission to publication in under 10 minutes when clean; unlock applies
immediately; AggregateRating appears only at 5 or more native reviews; all policy pages linked from the form.

### Phase 4: Growth engine (8 days)
Tasks: W15 Search Console ingest; W16 proposals; W12 refresh; W13 alerts and email; W18 link health; W22
first data study; W17 social autopilot with approval queue; Klaviyo flows.
Acceptance: growth panel live with daily data; first study published with CSV; first 30 days of posts
approved from the queue; alerts firing on real price events.

### Phase 5: Affiliate activation and expansion (ongoing from month 2)
Tasks: skeleton and policies live, apply to Sovrn, FlexOffers, Impact, Awin, CJ and brand programs; offers
and promo codes loaded; `/go/` redirect and click logging; conversions ingest; move Vercel and Supabase to
paid tiers; batch two (toppers, pillows), batch three (sheets, comforters), batch four (bases, kids, cribs).
Acceptance: first tracked commission; offer coverage on 90 percent of indexable models; batch expansion
following the protocol.

### Phase 6: Payments and premium (6 days, when traffic justifies)
Tasks: Stripe Checkout, Billing, Portal, Tax; webhooks; trial; founding-member price; reports; brand portal
billing.
Acceptance: subscribe, upgrade, cancel, and refund flows tested in Stripe test mode; reconciliation job
passes.

### Phase 7: Scale and B2B (later)
Tasks: brand accounts and analytics, data licensing exports, public API, second category families, second
brand evaluation.

## 20. Scaling path

| Trigger | Change |
|---|---|
| Database over 4 GB or ranking-page queries over 500 ms | Supabase compute upgrade; partition `observations`, `price_points`, `search_console_daily` by month |
| n8n executions over 50k per month or median step over 2 minutes | n8n queue mode with workers, or move hot workflows to edge functions |
| Search latency over 300 ms | Typesense or Meilisearch |
| More than three AI providers | Gateway (OpenRouter or Vercel AI Gateway) |
| International sales | Merchant of Record |
| Review volume over 1,000 per day | Dedicated moderation model tier; human moderator contract |
| Categories beyond sleep | Same schema; new category family, rubric, and collectors; consider a second brand only when the domain's topical authority would be diluted |

## 21. Risks and mitigations

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Google treats the site as scaled content | Medium | Severe | Index gate, small batches, owned data, native reviews, methodology and author, no head terms early |
| Amazon acts against collection | Low to medium | Medium | Managed provider, logged-out collection, no Associates account, derived data only; switch to Walmart, YouTube, editorial if blocked |
| Empty-page perception at launch | High if unmanaged | High | Intelligence layer seeds every indexed page; native reviews grow behind it |
| Fake or incentivized-looking reviews | Medium | High | Verification tiers, disclosure, moderation, brand-affiliation labeling, no sentiment conditioning |
| Affiliate reversals and cash lag | High | Medium | Model 20 percent reversal and 120-day lag; diversify programs; brand codes |
| Incumbent lab testers dominate head terms | High | Medium | Compete on owner data and comparisons; head terms last |
| Operator overload | Medium | High | Bulk queues, budget caps, weekly routine under four hours, kill switches |
| AI cost drift | Medium | Medium | Per-scope budgets, batch and caching, hashes to skip unchanged work |
| Vendor changes | Medium | Medium | Adapters isolated; data in Postgres; migrations in git |
| Legal claims over a review | Low | Medium | Section 230 posture, Terms, moderation records, dispute process, DMCA agent |

## 22. KPIs and targets

Tracked in mission control from day one:

| KPI | Q1 after launch | Q2 | Q4 |
|---|---|---|---|
| Indexable URLs | 90 to 150 | 250 to 400 | 700 to 1,000 |
| Organic sessions per month | 2k to 8k | 10k to 30k | 80k to 200k |
| Native reviews (cumulative) | 300 | 1,500 | 8,000 |
| Reviewer unlock conversion (visitors to review) | 0.3% | 0.5% | 0.8% |
| Email list | 500 | 3,000 | 25,000 |
| Tracked affiliate sales per month | 5 to 20 | 40 to 120 | 200 to 600 |
| Cost per indexable page (all-in) | under $5 | under $3 | under $2 |
| Operator hours per week | under 6 | under 4 | under 4 |

## 23. Open decisions and assumptions

Decided: niche (sleep), brand (RateMyBed), platform model, public reviews with analytics unlock, seeding by
collected reviews plus founding campaign, no Amazon Associates while collecting, n8n as orchestrator,
Supabase, Next.js on Vercel, Stripe later, review incentive terms, zero-spend build.

Open: n8n during build (Docker locally versus Railway); operator hours per week and target date; unlock
duration (proposed 12 months, renewable); budget ceiling for collection in month one (proposed $50); whether
the operator or counsel finalizes the legal documents; Discord for alerts (later).

Assumptions to verify before Phase 1: Apify actor pricing for Amazon and retailer reviews; Walmart API
review text availability; current affiliate program rates; keyword volumes from a paid tool; Vercel Hobby
non-commercial rule timing (move to Pro before the first affiliate link).

## Appendix A: document index

- `CLAUDE.md`: conventions every session must follow.
- `docs/01`: tooling audit and first critique. `docs/02`: decisions log. `docs/03`: software proposal v0.1
  (superseded where this plan differs). `docs/04`: evidence strategy. `docs/05`: source plans and first
  niche analysis. `docs/06`: niche and attribution. `docs/07`: niche rankings v3. `docs/08`: mattress
  comparison model. `docs/09`: sleep niche plan. `docs/10`: SEO and growth plan. `docs/11`: RateMyBed
  platform decision. `docs/12`: seeding decision.
- `docs/research/`: Google policy, legal sources, affiliate and pricing, stack pricing, review evidence
  channels, affiliate niches, niche batches, keyword volumes, sleep sub-categories.

## Appendix B: glossary

Observation: one paraphrased claim about one product on one criterion from one source, with a pointer.
Native review: a review submitted on this site by a verified owner. Data card: the objective facts block
(specs, warranty, certifications, price). Index gate: the rule that decides whether a page is indexable.
Shrunk mean: a rating average pulled toward the category prior in proportion to how few ratings exist.
Unlock: the 12-month analytics access granted for one verified review. Batch: a set of URLs released to
the index together and measured as a group.

# Software Proposal: Product Intelligence Platform

Version 0.1, 2026-09-29. Status: FOR REVIEW. Nothing has been built or installed.
Inputs: `01-tooling-audit-and-clarifying-questions.md`, `02-decisions-log.md`, `research/*.md`.

---

## 1. Executive Summary

Build a provenance-first product intelligence database on plain Postgres (Supabase), orchestrated by a thin
n8n layer, rendered by an AI-maintained Next.js site on Vercel, and operated from an admin area inside that
same site. Start with 10 washing machines, using free, public-domain federal datasets (ENERGY STAR, Department
of Energy) as the authoritative spec spine, YouTube and retailer APIs for evidence, and editorial sites for
context. Publish few, deep, evidence-rich pages behind a completeness gate to stay on the right side of
Google's scaled-content policy. Build accounts and entitlements from day one, but sell nothing until the free
site has traffic. The first paid test is personalized weighted rankings plus saved comparisons. The build
phase runs at zero dollars per month; the first unavoidable costs (about $50 to $70 per month plus AI usage)
trigger when affiliate links go live.

Three findings from research change the original brief:

1. Reddit cannot be ingested legally at MVP scale. The free API is non-commercial only and the commercial
   license reportedly starts around $12,000 per month. The MVP links to Reddit threads as pointers and defers
   ingestion until either an approved application or revenue justifies a license.
2. Amazon's Product Advertising API is retired. Its replacement requires roughly ten qualifying sales in the
   prior thirty days, so a new site cannot show live Amazon prices or images on day one.
3. Google's 2025 and 2026 updates specifically punished aggregator and thin-affiliate patterns and rewarded
   sites with owned data. The site must be built as a data publisher, not a page factory.

## 2. Product Requirements

Consumer (free): browse category families and categories; search products; view product pages with
overall score, criterion scores, evidence, pros and cons, specs, price and availability by retailer with
freshness, alternatives; view category ranking pages with methodology; compare up to three products; follow
affiliate links with disclosure.

Consumer (premium, later): weighted personal rankings; unlimited and saved comparisons; saved products and
lists; price and ranking alerts by email; weekly digest; full evidence explorer; score and price history;
in-depth research reports; later a usage-capped research assistant.

Operator (admin): mission-control dashboard; pipeline runs per category and product; draft review with
evidence inspection, edit, approve, reject, reprocess, bulk actions; entity merge and split queue; change
history with rollback; cost and usage ledger; affiliate link health; publishing queue; later subscription
metrics.

System: lifecycle state machine per product; category rubric versioning; deterministic objective scoring;
structured AI subjective assessment with evidence floors; weekly incremental refresh; provenance on every
fact; raw content retention rules per source; budget caps per category and per day; audit log of all
material changes; model registry so AI providers are swappable.

## 3. Business Model

Revenue streams in order of activation:
1. Affiliate commissions (Phase 5): sub-affiliate network on day one, direct retailer and brand programs as
   traffic allows, Amazon Associates once pages carry original commentary and sales thresholds are reachable.
2. Premium subscription (Phase 6, after traffic): monthly base price, annual discount, seven-day trial with
   card on file, founding-member annual price.
3. One-time reports: single-product report with price and score history, or partial category report with
   the full version reserved for subscribers.
4. B2B data access (later): read-only API keys over the same database for brands, retailers, and analysts.

Unit economics targets: affiliate revenue per thousand visitors is low for appliances (see research), so the
washer category is a data proof, not a revenue proof. Robot vacuums are the recommended revenue test.

## 4. Affiliate Monetization Strategy

- Day one: Sovrn Commerce (about 24-hour approval, covers Walmart, Home Depot, Lowe's, brand stores; keeps
  roughly a quarter of commissions). Apply to Impact marketplace in parallel for Walmart, Home Depot, Lowe's.
- Brand direct: LG via CJ reportedly pays up to 12 percent on appliances, the best rate found; apply early.
- Amazon Associates: apply once the site has a privacy policy, disclosure, and pages with original analysis
  (required by the April 2026 agreement). Expect three sales in 180 days to keep the account. Use text links
  until Creators API eligibility is reached; never scrape Amazon.
- Compliance: FTC-style disclosure near every affiliate link, not only in the footer; Amazon's exact wording
  on every page with Amazon links; rel="sponsored" on all affiliate links; prices shown with source and
  timestamp; no cloaking that hides the referrer.
- Architecture: every product has zero or more `affiliate_offers` (merchant, destination URL, network,
  price, availability, last verified). The site renders offers from the database, never hardcoded links, so
  merchants can be added or swapped without touching pages.

## 5. Subscription Monetization Strategy

Comparables: Consumer Reports $10 monthly or $39 annual digital; Wirecutter $5 per four weeks or $40 annual;
RTINGS about $10 monthly or $45 annual (unverified); Which? about £9 to £12 monthly. Paid tiers in this
market gate depth of data; price tracking and alerts are free everywhere.

Recommended pricing to test (ranges, not final): Premium at $6 to $9 per month, $49 to $69 per year (about
two months free), founding-member annual at the bottom of the range, seven-day trial with card required.
A Pro tier at $19 to $29 per month for exports, reports, and higher assistant limits is a later experiment.
One-time reports at $4 to $12.

Recurring value thesis (your D20): the subscription is to the application, not to a product. Recurring
value comes from a weekly personalized digest (new products, price drops, ranking changes across saved
categories), saved research state, and depth on demand. Alerts are the retention mechanism.

## 6. Free vs Premium Feature Strategy

| Capability | Free | Premium | Pro (later) |
|---|---|---|---|
| Category rankings, product pages, scores, evidence summaries | Full | Full | Full |
| Comparisons | Up to 3 products, not saved | Unlimited, saved | Shared and exported |
| Personalized weighted rankings | Preview only (one slider, not saved) | Full, saved per category | Full |
| Saved products and lists | 5 items | Unlimited | Unlimited |
| Price and ranking alerts | None | Email alerts, weekly digest | Same, faster cadence |
| Evidence explorer (all observations by source and criterion) | Top 5 per criterion | Full | Full |
| Score and price history | Last 30 days | Full history | Full plus CSV |
| Research assistant | None | Capped queries per month | Higher cap |
| Reports | None | Included with limits | Included |

Principle: nothing that makes the free site worse for SEO or trust is gated. Rankings, scores, and the
rationale stay public and indexable.

## 7. MVP Definition

Proves the 20 capabilities in the brief on washing machines with 10 products:
category creation, rubric versioning, discovery, entity resolution, spec collection from authoritative
sources, evidence from several legal sources, provenance, objective scoring, structured subjective
assessment, rankings, product pages, category page, affiliate destinations, drafts, admin queue, approval,
publishing, refresh, change log, rollback. Accounts and entitlement tables exist; checkout is not enabled.

Out of MVP: Reddit ingestion, Amazon API data, the research assistant, payments, category auto-discovery
opportunity scoring, X and TikTok.

## 8. Recommended No-Code / Low-Code Stack

| Role | Choice | Type | Why | Operator burden | Lock-in | Outgrow path |
|---|---|---|---|---|---|---|
| Database, auth, storage, cron, queues, vectors | Supabase | Low-code | Plain Postgres you can query in a table editor or SQL; pgvector for AI-native search; MCP for agent access; free during build | Low; dashboard-driven | Low (pg_dump) | Managed Postgres (Neon, RDS) with the same schema |
| Orchestration | n8n, self-hosted (Docker locally during build, then Railway or a small VPS) | No-code | Visual workflows you can read; native Anthropic, OpenAI, OpenRouter, Postgres, Supabase nodes; error workflows | Medium; one container to keep alive | Low (JSON export in git) | Supabase-native pg_cron plus edge functions for hot paths; queue mode for scale |
| Site and admin | Next.js on Vercel, AI-maintained | Code (you never edit) | Only option that does SEO at scale, structured data, ISR, and admin in one deploy | Low; git push deploys | Low (standard framework) | Same framework on any host |
| AI | Anthropic direct (Haiku 4.5 for extraction and classification, Sonnet 5.5 for rubric and editorial) with batch and caching; OpenAI or xAI added through a model registry table | Config | Avoids gateway fees; provider swap is a table row plus an n8n credential | Low | None | OpenRouter or Vercel AI Gateway if routing gets complex |
| Collection | Official APIs (ENERGY STAR, DOE, YouTube, Best Buy, Walmart); Firecrawl free tier for manufacturer and editorial pages; Serper or Brave for discovery | Low-code | Legal by construction; free tiers cover 10 products per day | Low | Low (adapters are thin) | Apify actors or a second provider |
| Email | Resend (3,000 per month free) | No-code | Magic links and alerts; good API | Low | Low | Postmark or SES |
| Auth | Supabase Auth (magic link, Google) | No-code | Managed, included, 50k MAU free | Low | Low (standard JWT, exportable users) | Clerk if prebuilt UI is wanted |
| Payments (Phase 6) | Stripe Checkout, Billing, Customer Portal, Tax for US-only; Polar if international | No-code checkout | Lowest fees domestically; instant onboarding | Low | Medium (subscriptions are portable with effort) | Paddle or Stripe Managed Payments for global MoR |
| Monitoring | Sentry free, Better Stack free, n8n error workflow, cost ledger in Postgres | No-code | Enough for one operator | Low | Low | Paid tiers |

Vendor count at MVP: Supabase, Vercel, n8n host, Anthropic, Firecrawl, Serper or Brave, Resend. Seven, all
free during build.

## 9. System Architecture

```
                 +--------------------+        +----------------------+
  Sources  --->  |  Collectors (n8n)  | -----> |  raw_content (TTL)   |
  (APIs,         |  one per adapter   |        |  source_documents    |
   fetches)      +--------------------+        +----------+-----------+
                                                          |
                 +--------------------+        +----------v-----------+
                 |  AI tasks (n8n +   | <----> |  observations        |
                 |  model registry)   |        |  spec_values         |
                 +--------------------+        |  assessments         |
                                               +----------+-----------+
                 +--------------------+                   |
                 |  Deterministic     | <-----------------+
                 |  scoring (SQL fns) | -----> scores, rankings, drafts
                 +--------------------+
                          |
   +----------------------v-----------------------+
   |  Supabase Postgres: single source of truth   |
   |  pg_cron schedules, pgmq queues, RLS, audit  |
   +----------+-----------------------+-----------+
              |                       |
   +----------v---------+   +---------v-----------+
   |  Next.js site      |   |  Next.js /admin     |
   |  (public, ISR)     |   |  (role-gated)       |
   +--------------------+   +---------------------+
```

Rules: all state lives in Postgres. n8n workflows are stateless step executors triggered by queue messages or
cron. Deterministic logic is SQL functions or small edge functions with tests. AI calls go through one n8n
sub-workflow that reads the model registry. The site reads published views only.

## 10. Data Architecture

Layers:
1. Raw layer: `source_documents` (URL, source type, fetched at, content hash, robots and license flags,
   `expires_at` from the source's retention rule) and `raw_content` (text or JSON, deletable). YouTube rows
   expire at 30 days; Amazon price rows at 1 hour; manufacturer facts never.
2. Fact layer: `spec_values` (product, attribute, value, unit, normalized value, source document, confidence,
   collected at, verified at, workflow run). Multiple values per attribute are allowed; a resolver picks the
   canonical value by source authority (federal dataset > manufacturer > retailer).
3. Evidence layer: `observations`: one atomic claim about one product on one criterion from one source, with
   polarity, strength, excerpt (only where storable), pointer to the source document, extraction model, and
   run. This is the durable asset; it survives raw-content deletion because the pointer and the derived claim
   are ours.
4. Judgment layer: `assessments` (per product, criterion, rubric version: score, rationale citing observation
   IDs, evidence count, confidence, model, run) and `scores` (objective and subjective, per rubric version).
5. Presentation layer: `product_pages` and `category_pages` (generated content blocks, status draft or
   published, version), rendered by the site.
6. Commerce layer: `affiliate_offers`, `price_points` (time series).
7. Account layer: users, profiles, saved items, alerts, plans, subscriptions, entitlements, payment records.
8. Operations layer: `pipeline_runs`, `pipeline_steps`, `ai_calls` (tokens and cost), `change_log`, `budgets`.

Provenance is a column set, not an afterthought: every fact and observation row carries source document,
collected at, verified at, confidence, and producing run.

## 11. Database Schema

Core tables (columns abbreviated; all have `id uuid`, `created_at`, `updated_at`):

- `category_families` (slug, name). `categories` (family, slug, name, status, config jsonb:
  max_products, max_sources_per_product, daily_ai_budget_usd, refresh_interval_days).
- `rubrics` (category, version, status, weights jsonb). `rubric_criteria` (rubric, key, name, type
  objective|subjective, weight, normalization jsonb: method, min, max, direction, evidence_floor).
- `manufacturers` (name, slug, website).
- `products` (category, manufacturer, canonical_name, model_family, status lifecycle enum, published_at,
  completeness_score). `product_variants` (product, model_number, color, upc, gtin, region).
  `product_identifiers` (variant, type, value, source). `product_aliases` (product, alias, source).
- `entity_candidates` (raw name, model, source, proposed product, confidence, decision pending|merged|split|new,
  decided_by, decided_at) for the merge and split queue.
- `sources` (key, name, type, license_class, retention_days, allowed_store_excerpt bool).
  `source_documents` (source, product nullable, url, title, fetched_at, content_hash, expires_at, robots_ok).
  `raw_content` (source_document, body, mime, expires_at).
- `attributes` (category, key, name, unit, datatype). `spec_values` (product or variant, attribute, value_text,
  value_num, unit, normalized_num, source_document, confidence, collected_at, verified_at, run, is_canonical).
- `observations` (product, criterion nullable, source_document, kind praise|complaint|fact|opinion,
  polarity, strength, excerpt, pointer jsonb, extracted_by_model, run, superseded_by).
- `assessments` (product, rubric, criterion, score, rationale, evidence_ids uuid[], evidence_count,
  confidence, model, run). `scores` (product, rubric, criterion nullable, kind objective|subjective|overall,
  value, computed_at, run). `rankings` (category, rubric, product, rank, score, computed_at).
- `product_pages` (product, version, status, blocks jsonb, generated_by, approved_by, published_at).
  `category_pages` (category, version, status, blocks jsonb).
- `merchants` (name, network, program_id). `affiliate_offers` (product or variant, merchant, url, price,
  currency, availability, price_as_of, last_verified_at, active). `price_points` (offer, price, observed_at).
- `users` (Supabase auth), `profiles` (display name, role), `saved_items`, `saved_comparisons`,
  `user_weightings` (user, category, rubric, weights jsonb), `alerts` (user, type, target, threshold, active),
  `alert_events`.
- `plans` (key, name, monthly_price, annual_price, entitlements jsonb). `subscriptions` (user, plan, status,
  period_start, period_end, trial_end, cancel_at, processor, processor_subscription_id).
  `entitlements` (user, key, value, source, expires_at). `payment_customers` (user, processor, customer_id).
  `payment_events` (processor, event_id, type, payload, processed_at). `usage` (user, feature, period, count).
- `pipeline_runs` (category, product nullable, workflow, status, started_at, finished_at, error, cost_usd).
  `pipeline_steps` (run, step, status, input_hash, output_hash). `ai_calls` (run, task, provider, model,
  input_tokens, output_tokens, cached_tokens, cost_usd, latency_ms). `budgets` (scope, period, limit_usd,
  spent_usd). `change_log` (table, row_id, field, old_value, new_value, actor_type, actor_id, reason,
  source_document, run, reverted_by). `models` (task, provider, model_id, max_cost_per_call, active).

Indexes on every foreign key, on `observations(product, criterion)`, `spec_values(product, attribute)`,
`price_points(offer, observed_at)`, plus a pgvector column on `observations` and `product_pages` for semantic
search. Row Level Security on all account tables. Published data exposed through views the site reads.

## 12. Authentication Architecture

Supabase Auth with email magic link or OTP and Google OAuth. Profiles row created by trigger on signup with
role `user`; the operator's account is set to `admin` by migration. Admin routes check the role server-side.
JWTs carry only identity; entitlements are always read from the database, never from the token. Users are
exportable, so moving to Clerk later is a migration, not a rewrite.

## 13. Subscription and Entitlement Architecture

- `plans` define entitlement keys and limits. `subscriptions` mirror processor state through webhooks.
- A SQL function `effective_entitlements(user_id)` combines the active subscription's plan, any manual grants
  in `entitlements` (comps, trials, founders), and expiry, returning a flat key-value set.
- The site and n8n call that function; the frontend never decides paid status.
- Webhook handler (edge function) writes `payment_events` idempotently by event ID, then updates
  `subscriptions`. A nightly reconciliation job compares processor state with local state and flags drift.
- Usage limits (assistant queries, alerts) are counted in `usage` and checked before the action.
- Changing processor means new rows in `payment_customers` and a new webhook mapping; entitlement logic is
  untouched.

## 14. Payment Processor Comparison

| Platform | Best For | No-Code Ease | Tax Handling | Subscription Features | Cost Structure | Scalability | Lock-In | Recommendation |
|---|---|---|---|---|---|---|---|---|
| Stripe (Checkout, Billing, Portal, Tax) | US-first SaaS wanting lowest fees and best tooling | High: hosted checkout, payment links, portal | Calculates and collects; you register and remit | Full: trials, coupons, proration, pause, dunning, portal | 2.9% + 30c, +0.7% Billing, +0.5% Tax | Excellent | Medium | Recommended for US-only launch |
| Stripe Managed Payments | Stripe users wanting Merchant of Record | High, same surface | Full MoR | Same as Stripe | about 6.4% + 30c (7.1% with Billing) | Excellent | Medium | Second choice if going international while on Stripe |
| Paddle | Global SaaS wanting mature MoR | High: hosted checkout, portal | Full MoR | Full | 5% + 50c | Excellent | Medium-high | Alternative MoR; slow, strict onboarding |
| Polar.sh | Indie developers wanting instant global MoR | High | Full MoR | Trials, discounts, portal, webhooks | 5% + 50c (3.8% + 40c on $20 plan) | Good, young | Medium | Best instant MoR for a sole proprietor going international |
| Lemon Squeezy | Legacy users | High | Full MoR | Full | 5% + 50c | Uncertain | High | Do not start here (acquired, signups gated) |
| Gumroad | Digital products | High | MoR | Weak SaaS features | 10% + card fees | Limited | Low | Not suitable |

Payment processor versus Merchant of Record: a processor moves money and leaves you as the seller of record,
responsible for sales tax and VAT registration, collection, remittance, invoices, and chargebacks. A Merchant
of Record resells your product to the customer, so it is the legal seller, files the taxes in every
jurisdiction, and handles invoices and disputes, in exchange for roughly two to three percentage points more.

## 15. Recommended Payment Processor

1. MVP (US-only, activated in Phase 6): Stripe with Checkout, Billing, Customer Portal, and Stripe Tax.
   Reason: instant onboarding, lowest fees, best webhook tooling, Stripe plugin available in this environment.
   As a US sole proprietor selling US-only digital subscriptions at launch volumes, you will typically have
   nexus only in your home state; Stripe Tax tells you when thresholds elsewhere approach.
2. Second choice: Polar, if you decide to accept international customers at launch. Instant onboarding,
   Merchant of Record, built on Stripe.
3. Switch trigger: meaningful non-US revenue (registration obligations in several jurisdictions) means moving
   to Stripe Managed Payments or Paddle. Because entitlements are local, the switch is a webhook remap and a
   customer migration, not a product change.
4. Merchant of Record makes sense for this business the moment it sells outside the US. Before that, the
   premium is not worth paying.

## 16. Automation Architecture

Three tiers, as in the brief:
- Collectors: one n8n workflow per source adapter, each writing `source_documents` and `raw_content` in the
  same shape. Adapters: energy_star, doe_ccms, manufacturer_page, youtube, bestbuy, walmart, editorial_page,
  discovery_search, bluesky (optional).
- Deterministic automations: SQL functions for normalization, objective scoring, weighting, ranking, change
  detection (content hashes), refresh scheduling (pg_cron), budget checks, entitlement resolution, alert
  matching. n8n only calls them.
- AI tasks: rubric drafting, spec extraction from pages, observation extraction and classification,
  subjective assessment, editorial generation, QA review, entity adjudication. Each is one n8n sub-workflow
  with a fixed JSON output schema validated before writing.

Queue design: `pgmq` queues per stage (discover, identify, collect, extract, assess, score, draft, qa).
Each product moves by enqueueing the next stage on success. n8n polls queues on a schedule; failures go to
a dead-letter queue visible in admin. Every step records `pipeline_steps` with input and output hashes so
reruns skip unchanged work.

## 17. AI Architecture

- Model registry table maps task to provider, model, max cost per call, and whether batch is allowed. MVP
  mapping: extraction and classification on Haiku 4.5 via Batch API with prompt caching; rubric, assessment,
  and editorial on Sonnet 5.5; QA on a different model family (an OpenAI small model) once an OpenAI key
  exists; math in SQL, never in a model.
- Prompts are versioned files in the repo under `/prompts`, loaded into a `prompt_versions` table; every
  `ai_calls` row records the prompt version.
- Structured outputs: every task returns JSON validated against a schema; invalid output retries once, then
  fails the step.
- Cost control: `ai_calls` records tokens and cost; a per-category daily budget and a global daily budget are
  checked before each call; exceeding a budget pauses the queue and raises an admin alert.
- Abstraction depth for MVP: the registry table plus one `call_llm` sub-workflow. No custom gateway. Add
  OpenRouter or Vercel AI Gateway only if more than three providers are in use.
- Evals: a small fixture set per task (10 to 20 labeled examples) run before changing a prompt or model.

## 18. Source Adapter Architecture

Each adapter declares: license class, retention days, whether excerpts may be stored, rate limit, cost per
call, and required credentials. The `sources` table holds these; collectors read them.

MVP adapters and what they contribute for washers:
| Adapter | Contributes | Legal basis | Cost |
|---|---|---|---|
| ENERGY STAR (SODA API) | Model numbers, capacity, IMEF, IWF, energy and water use, UPCs, brand | Public domain | $0 |
| DOE CCMS | All certified models including non-ENERGY STAR; capacity and efficiency | Public domain | $0 |
| Manufacturer pages and PDFs (Firecrawl) | Dimensions, cycles, features, warranty, MSRP; facts only, linked | Facts not copyrightable; robots respected | $0 (free credits) |
| YouTube Data API | Review videos (title, channel, stats) and public comments as evidence; refreshed or deleted within 30 days; observations retain pointers | API terms | $0 |
| Best Buy API | Price, availability, rating average and count with attribution | API terms | $0 |
| Walmart Affiliate API | Price, availability, rating aggregates | Impact account | $0 |
| Editorial and blog reviews (discovery via Serper or Brave, then direct robots-respecting fetch) | Score, verdict, short attributed quote, link | Fair-use citation | $0 to small |
| Reddit | Thread links as pointers only; no ingestion | Linking is fine; ingestion deferred | $0 |
| Bluesky firehose (optional) | Brand-level sentiment mentions | Public API with delete propagation | $0 |

Discovery verification (your D11): for each product, the discovery workflow searches model number and name
variants, fetches candidate pages, and an AI task confirms the page actually discusses that product before
any observation is extracted. Pages failing verification are recorded as rejected candidates.

Reddit alternatives: (1) apply for Reddit Data API access under the Responsible Builder Policy describing the
use and see whether approval comes with acceptable terms; (2) human-in-the-loop: you read threads and enter
observations by hand in the admin, which is legal and feasible at 10 products; (3) license later when revenue
supports it. The schema treats Reddit like any other source, so turning it on later is configuration.

## 19. Product Lifecycle

States: discovered, identified, collecting, collected, specs_verified, evidence_analyzed, scored, qa_passed,
qa_failed, draft, in_review, approved, rejected, published, monitoring, retired. Transitions are enforced by a
SQL function that also writes `change_log`. Each state has an owning workflow and a maximum age; products
stuck past it appear in the admin as stale.

## 20. Category Lifecycle

States: proposed, rubric_drafting, rubric_review, active, paused, retired. Creating a category: operator or
discovery job proposes it; rubric drafting task researches criteria and proposes weights and normalization;
operator approves rubric v1.0; product discovery runs within the category budget. Rubric changes create a new
version and enqueue rescoring for all products in the category; old scores remain for history.
`new_categories_per_day` and per-category budgets live in a `settings` table.

## 21. Ranking and Scoring Architecture

- Objective criteria: `spec_values.normalized_num` computed by the rubric's normalization method (min-max
  within category, log scale, or threshold bands, with direction). Score = normalized value times 10.
- Subjective criteria: the assessment task receives only observations for that product and criterion, must
  cite observation IDs, and returns score, rationale, and confidence. Below the criterion's evidence floor the
  criterion is unscored and the overall score is computed on available weight with a visible confidence flag.
- Overall = weighted sum of available criteria, renormalized to available weight, with a coverage penalty if
  coverage is under a threshold. Rankings are recomputed by a SQL function on any score change.
- Personalized rankings (premium) reuse the same criterion scores with user weights; no AI cost.
- Every score row records rubric version and run, so any ranking can be explained and reproduced.

## 22. Admin Dashboard Architecture

Routes under `/admin` in the Next.js app, role-gated:
- Today: active category, counts by state, drafts ready, failures, spend today versus budget.
- Pipeline: runs and steps with errors, retry, dead-letter queue.
- Drafts: list, filters, bulk approve or reject, page preview, evidence panel per criterion, inline edit
  of blocks, reprocess.
- Entities: merge and split queue with side-by-side candidates and identifiers.
- Changes: change log with diff and one-click revert.
- Sources: adapter health, last run, quota use, documents expiring.
- Affiliate: offers, last verified, broken links.
- Costs: AI calls by task and model, daily trend, budget settings.
- Later: subscriptions, MRR, churn, feature usage.
Supabase Studio remains available for raw table access and SQL.

## 23. Consumer Website Architecture

Next.js App Router on Vercel with Incremental Static Regeneration. Routes: `/`, `/[family]/`,
`/[family]/[category]/` (ranking page), `/[family]/[category]/[product-slug]/`, `/compare/[a]-vs-[b]/`,
`/methodology/`, `/about/` (author page), `/search`. Search: Postgres full-text plus pgvector at MVP.
Pages read published views only. Structured data: Product snippet, Review with author and reviewRating,
positiveNotes and negativeNotes, BreadcrumbList, Organization, Person. Affiliate links rel="sponsored" with
disclosure adjacent. Price shown with source and "as of" time.

## 24. Affiliate Architecture

`merchants` and `affiliate_offers` as in section 11. An offer resolver picks which offers to display by
availability, freshness, and merchant priority. Link verification runs weekly (HTTP status and redirect
target). Clicks are logged through a first-party redirect endpoint (`/go/[offer]`) so click-through can be
measured without cloaking the destination (the redirect is transparent and marked sponsored).

## 25. Draft and Publishing Workflow

Draft creation writes a `product_pages` row with status draft and a QA report. Admin review shows the page,
evidence, QA findings, and completeness score. Approve sets published, records approver, and triggers ISR
revalidation. Reject records a reason and optionally enqueues reprocessing. Bulk actions apply to filtered
sets. Auto-publish is a per-category setting, off by default, with a minimum QA score.

## 26. Change History and Rollback

Triggers on tracked tables (`products`, `spec_values`, `scores`, `assessments`, `product_pages`,
`affiliate_offers`, `rubrics`) write `change_log` rows with old and new values, actor (user, workflow, or
model), reason, source document, and run. Revert writes the old value back through the same path, creating a
new log row that references the reverted one. Bulk revert by run ID undoes everything a failed run wrote.

## 27. Weekly Refresh System

pg_cron enqueues products whose `next_refresh_at` has passed. The refresh workflow runs cheap checks first:
price and availability by API, content hash of manufacturer pages, new YouTube results since last run, new
editorial mentions since last run. Only changed inputs trigger re-extraction; only changed observations
trigger re-assessment of the affected criteria; only changed scores trigger re-ranking and page regeneration.
Successor detection compares new ENERGY STAR and DOE rows against known model families.

## 28. User Account System

Signup by magic link or Google. Profile with optional household fields (budget range, household size,
priorities) stored in `profiles.preferences` for later personalization. Saved items, comparisons, and
weightings tied to the user. Account deletion cascades and is logged.

## 29. Saved Products and Alerts Architecture

`alerts` rows define type (price_below, price_drop_pct, rank_change, new_product_score_above), target
(product or category), and threshold. A nightly SQL job matches alerts against `price_points` and
`rankings` changes, writes `alert_events`, and an n8n workflow sends batched emails through Resend. The weekly
digest is the same mechanism with a broader query.

## 30. Error Handling

Every n8n workflow has an error workflow that writes `pipeline_runs.error` and moves the message to the
dead-letter queue. Retries: three with backoff for network errors; none for schema-validation failures, which
require a prompt fix. Stale-state detection in admin. Nothing is silently dropped.

## 31. Monitoring and Alerts

Sentry for site and edge function errors. Better Stack uptime on the site and the n8n instance. An n8n
"daily health" workflow emails a summary (later Discord): runs, failures, spend, documents expiring, drafts
waiting. Budget breaches and dead-letter growth alert immediately.

## 32. Cost Controls

Per-category and global daily AI budgets in `budgets`, checked before each call. Batch API for anything not
latency-sensitive. Prompt caching for shared context. Content hashes to skip unchanged work. Per-task cost
ceilings in the model registry. Free-tier quotas tracked per adapter with soft stops at 80 percent. A monthly
cost report by task and category in admin.

## 33. Security

Secrets only in Supabase Vault, n8n credentials, and Vercel environment variables; never in the repo. Row
Level Security on all user data. Admin role checked server-side. Webhooks verified by signature and
idempotent by event ID. Supabase MCP used against a dev project; production changes through migrations in
git. `/security-review` run before every merge. Least-privilege API keys with spend limits at each vendor.
Public site serves published views only; no direct table access from the browser.

## 34. SEO Architecture

Strategy built from the research findings:
1. Position as a data publisher. Every indexed page exposes owned data: computed scores with methodology,
   normalized spec comparisons, evidence counts by criterion, price history, and freshness. That is the
   "unique data" test every practitioner and Google's own review guidance point to.
2. Index gate. A page is indexable only when its completeness score passes a threshold (specs coverage,
   evidence floor met on most criteria, at least N distinct sources, unique-content ratio above 50 percent).
   Everything else ships with noindex. The pipeline can run one category per day; publishing does not.
3. Launch small. Ten washer pages, one ranking page, three to five comparison pages, a methodology page, and
   an author page. Expand in batches only when Search Console shows indexation and impressions.
4. Who, How, Why. Real byline (Nicholas Moeller) and author page with credentials and sameAs links; a
   methodology page explaining scoring, sources, refresh cadence, and how AI is used and checked; an AI
   disclosure block on generated sections. Never present synthesized text as consumer reviews (FTC rule 465).
5. Structured data per section 23; never AggregateRating from third-party ratings.
6. Affiliate hygiene: rel="sponsored", disclosure near links, multiple sellers, original analysis on every
   page with Amazon links.
7. First-party only: no guest content, no sold subfolders, fresh domain rather than an aged one.
8. AI Overviews: expect halved click-through on top positions; optimize to be cited (clear entity data,
   quantitative claims, fresh updates) and build a direct channel from day one (email capture for alerts and
   digests, even before premium exists).
9. Site health: monitor per-category performance; noindex or prune weak categories quickly; a bad section
   drags the domain.
10. Depth over breadth in the first year; the March 2026 update rewarded owned data and punished generic
    aggregation.

## 35. Subscription Analytics

Stripe's built-in reporting covers MRR, churn, and failed payments at MVP scale. Local `subscriptions` and
`payment_events` allow cohort queries in SQL and admin charts. A dedicated analytics product (ProfitWell,
ChartMogul) is unnecessary below a few thousand subscribers.

## 36. Scaling Strategy

MVP (now to about 20 categories, 2,000 products, 500k observations): Supabase Pro on Micro or Small compute,
single n8n instance, Postgres full-text search, ISR.
Growth (to about 200 categories, 30k products, 10M observations): Supabase Medium or Large compute,
partition `observations` and `price_points` by month, n8n queue mode with workers or hot workflows moved to
edge functions, Typesense or Meilisearch for search, read replica for the site.
Scale (1,000+ categories, 100k+ products, 100M+ observations): dedicated managed Postgres, workers in code
(Node or Python) consuming the same queues, columnar analytics store for history, CDN-level caching, and a
public B2B API on the same schema.

## 37. MVP to Growth to Scale Migration Plan

Triggers: database over 4 GB or query latency over 500 ms on ranking pages moves to Growth; n8n executions
over 50k per month or median step latency over 2 minutes moves hot paths to edge functions; search latency
over 300 ms moves search out of Postgres; more than three providers moves AI calls behind a gateway; any
non-US revenue moves payments to a Merchant of Record. Each trigger is observable in the admin cost and
health pages. The schema does not change across tiers; only hosting and workers do.

## 38. Major Risks

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Google classifies the site as scaled content | Medium | Severe | Section 34; small launch; index gate |
| Evidence too thin without Reddit and Amazon reviews | High for washers | High | YouTube, editorial, retailer aggregates; evidence floors; human-entered observations; pick evidence-rich second category |
| Affiliate revenue too low on appliances | High | Medium | Robot vacuums as revenue test; LG direct program |
| Amazon Associates closed for lack of sales | Medium | Medium | Apply late, when traffic exists |
| Subscription willingness to pay unproven | High | High | Sequence after traffic; cheap first test; one-time reports; B2B path |
| AI extraction errors | Medium | Medium | Schema validation, QA model, evals, human review |
| Operator overload | Medium | High | Bulk actions, QA thresholds, budget caps, one weekly review rhythm |
| Vendor changes (terms, APIs) | Medium | Medium | Adapters isolated; sources table encodes terms |
| Free-tier pauses or limits | High | Low | Keep-alive ping; move to Pro when live |

## 39. Implementation Phases

Phase 0, Foundation: Supabase dev project, repo structure, migrations for the full schema, seed data,
conventions skill, n8n running locally, Anthropic key with a $20 monthly cap.
Phase 1, Data spine and pipeline: ENERGY STAR and DOE ingestion, entity resolution with merge queue,
manufacturer spec extraction, YouTube and Best Buy adapters, discovery with verification, observation
extraction, washer rubric v1.0, scoring, ranking, QA, drafts for 10 washers.
Phase 2, Site and admin: public pages, ranking page, comparison, methodology, author page, structured data,
admin dashboard, draft review, publish, ISR.
Phase 3, Operations: weekly refresh, change log and rollback, cost ledger and budgets, health emails,
Sentry, uptime, Playwright checks.
Phase 4, Accounts and entitlements: auth, profiles, saved items, weighted rankings gated by entitlement,
email capture, digest skeleton; no checkout.
Phase 5, Second category and affiliate: robot vacuums, Sovrn and Impact links, click logging, Amazon
application when eligible; move Vercel and Supabase to paid tiers.
Phase 6, Payments: Stripe Checkout, Billing, Portal, Tax, webhooks, trial, founding-member price, alerts
live; only when traffic justifies it.

## 40. Estimated Complexity of Each Phase

| Phase | Complexity for one operator with AI assistance | Main effort |
|---|---|---|
| 0 | Low | Accounts and configuration |
| 1 | High | Adapters, extraction quality, rubric, entity resolution |
| 2 | Medium | Page templates, admin views, structured data |
| 3 | Medium | Refresh logic, rollback correctness |
| 4 | Low-Medium | Auth wiring, RLS, entitlement function |
| 5 | Low-Medium | New category config, affiliate signups, link plumbing |
| 6 | Medium | Webhook reliability, tax setup, trial flows |

## 41. Recommended First Build

Phase 0 plus the first half of Phase 1: the schema, ENERGY STAR and DOE ingestion for washers, entity
resolution into 10 canonical products with a merge queue, and one product carried end to end through spec
extraction, evidence collection from YouTube and one editorial source, scoring, and a draft page visible in
a minimal admin. This proves provenance, legality, cost per product, and the operator's review loop before
any page is public and before any money is spent.

---

## Open decisions for the operator

1. Reddit: accept deferring ingestion (link-only, optional human-entered observations, apply for API access)?
2. Second category: robot vacuums?
3. n8n hosting during build: Docker on your machine (free, needs Docker installed) or Railway (about $5 per
   month, zero setup)?
4. Raw content policy: confirm excerpts plus derived observations only.
5. Hours per week and target date (question 3 from round one): "hours per week" means how much time you
   can spend on reviewing drafts, resolving entity merges, and checking alerts once the system runs; "target
   date" means when you want the first washer pages live. These set how much manual review the MVP can
   assume and how aggressive the phase schedule should be.
6. Approval to proceed with the Step 0 sequence: enable the Supabase plugin against a new dev project, then
   create the conventions skill after this schema is approved.

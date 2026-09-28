# Design Round 1: Tooling Audit, Product Restatement, Critique, Clarifying Questions

Status: DISCUSSION DOCUMENT. Nothing has been installed, created, or built.
Date: 2026-09-28

---

## STEP 0 — Tooling audit

### What is already in this environment

Enabled skills (relevant): skill-creator (official), deep-research (official), mcp-builder (official),
claude-api (built-in reference for model IDs/pricing/caching), code-review, security-review, simplify,
session-start-hook, docs/xlsx/docx/pdf/pptx (document output), dataviz.

Connected MCP connectors: GitHub (scoped to this repo), ClickUp, Google Drive, Google Calendar, Klaviyo,
Shopify, Triple Whale, Commslayer. Gmail, Canva, Brandwise need reconnect. Most of these belong to the
AutoLinkr store and are not relevant to this project; ClickUp could optionally track build tasks.

Plugins enabled: none. Available in the Anthropic Directory (not enabled): Supabase (partner), Stripe
(partner), Apify (partner), Firecrawl (partner), Sentry (partner), Playwright (Microsoft), Exa (partner),
plus community SEO/security plugins.

Local toolchain: Node 22, Python 3.11, psql client, Docker, Chromium/Playwright preinstalled.
Not present: Supabase CLI, Vercel CLI, n8n, Stripe CLI. No n8n plugin or connector exists in the catalog.

Repository: empty, zero commits, branch claude/pensive-hopper-595065.

### Recommendation table

| Priority | Skill / Capability | Exists? | Purpose | MVP? | Recommendation |
|---|---|---|---|---|---|
| 1 | Supabase official plugin (skills: supabase, supabase-postgres-best-practices; MCP server) | Available, partner tier, not enabled | Schema/migration/RLS/auth/storage guidance; MCP can run SQL and migrations against a project | Yes | INSTALL NOW (once Supabase confirmed in Step 4) |
| 2 | skill-creator (Anthropic) | Enabled | Build the one custom skill we need | Yes | USE EXISTING |
| 3 | deep-research (Anthropic) | Enabled | Verify payment fees, affiliate terms, API terms, competitor pricing for the Step 5 proposal | Yes | USE EXISTING |
| 4 | GitHub MCP + code-review + security-review + session-start-hook (built-in) | Enabled | Repo operations, review of AI-generated code, CI setup | Yes | USE EXISTING |
| 5 | claude-api reference skill (built-in) | Enabled | Model selection, prompt caching, batch API for cost control | Yes | USE EXISTING |
| 6 | CUSTOM: `product-intel-conventions` (merges "Product Intelligence Data Architect" + "No-Code Architecture Reviewer") | Must create | Encodes approved schema, provenance rules, entitlement rules, n8n naming/error conventions, change-log rules, so every future session builds consistently | Yes | CREATE CUSTOM (after schema is approved, not before) |
| 7 | Stripe official plugin (skills + MCP + test-cards command) | Available, partner | Billing, Checkout, webhook and Customer Portal guidance | Phase 2 | CONSIDER LATER (only if Stripe/Managed Payments is chosen; unnecessary if Paddle) |
| 8 | Apify official plugin (MCP + skills) or Firecrawl plugin | Available, partner | Managed scrapers/crawlers for source adapters | Phase 1 build | CONSIDER LATER (choose collection provider first; install one, not both) |
| 9 | Sentry official plugin | Available, partner | Error monitoring for site + functions | End of Phase 1 | CONSIDER LATER |
| 10 | Playwright MCP (Microsoft) | Available, partner; Chromium already local | End-to-end checks of admin and site | End of Phase 1 | CONSIDER LATER |
| 11 | securitymaxxing, supabase-expert, seo-geo-consultant, SearchFit SEO, Qdrant, designs-drift, prism | Community tier | Overlap with built-ins or not needed (pgvector covers vectors; security-review covers audits) | No | DO NOT INSTALL |
| 12 | Runtime "skills": Category Rubric Architect, Product Entity Resolver, Source Provenance Validator, Affiliate Compliance Checker, Product Research QA, Ranking QA, Subscription Entitlement QA | N/A | These are pipeline prompts and validators that run inside the automation platform at runtime, not Claude Code skills | Rubric, Resolver, Research QA: MVP | CREATE as versioned prompt/validator assets in the repo (`/prompts`, `/validators`), not as skills |

Important distinction: of the nine custom skills listed in the brief, only two are development-time
capabilities (Data Architect, Architecture Reviewer), and they collapse into one conventions skill.
The other seven are runtime components of the product itself. They must live in the database/repo
as versioned prompts with evals, because they run in n8n or edge functions, not in this editor.

### Per-item detail for the recommended set

1. Supabase official plugin
   - Source: supabase-community/supabase-plugin (partner tier, directory reviewed).
   - Does: skills for Postgres best practices and Supabase patterns; MCP server that can list projects,
     apply migrations, run SQL, read logs.
   - Triggers: any schema, migration, RLS, auth, or storage work.
   - Executes actions: YES. The MCP can write to a live database.
   - Security: configure with a personal access token scoped to a DEV project; enable read-only mode for
     anything pointed at production; never store the PAT in the repo. Production schema changes go through
     migration files in git, applied by CI, not through the MCP interactively.
   - Overlap: none once supabase-expert is excluded.
   - Install: enable plugin in claude.ai plugin settings, then authenticate the MCP with a Supabase PAT.
2. skill-creator: enabled; no action.
3. deep-research: enabled; will be used in Step 5 to verify fees/terms rather than relying on memory.
4. GitHub MCP + review skills: enabled; run `/security-review` before every merge of generated code.
5. claude-api: enabled; used when designing the model gateway and cost caps.
6. Custom `product-intel-conventions` skill
   - Does: a single SKILL.md plus references (schema summary, naming conventions, provenance rules,
     entitlement rules, change-log rules, n8n workflow conventions, "never do" list).
   - Triggers: any work in this repo.
   - Executes actions: NO (instructions only).
   - Security: none beyond ordinary; it is text we control.
   - Timing: created after the Step 5 schema is approved, otherwise it encodes decisions not yet made.
7. Stripe plugin: partner tier; MCP can create products/prices/links in your Stripe account, so install
   with restricted API key in test mode first.
8. Apify or Firecrawl: both are remote MCPs running against paid accounts; both spend money per call.
   Set spend limits in the vendor dashboard before enabling.
9. Sentry: MCP reads issues; low risk.
10. Playwright MCP: drives a browser; keep it pointed at staging URLs.

### Recommended installation sequence (nothing done yet, awaiting approval)

1. Agree the stack decisions in Step 4 below.
2. Create a Supabase organization with a DEV project; enable the Supabase plugin against it.
3. Approve the Step 5 schema; then create `product-intel-conventions` with skill-creator.
4. When building collectors: pick Apify or Firecrawl, set vendor spend caps, enable that one plugin.
5. End of Phase 1: enable Sentry and Playwright.
6. Phase 2 (payments): enable Stripe plugin only if Stripe is the chosen processor.

Target footprint: 6 capabilities for MVP (items 1 to 6), 4 deferred. No community-tier plugins.

---

## STEP 1 — Restatement

You are building a product intelligence database, not a content site. The database accumulates
canonical products, variants, specifications with provenance, evidence observations extracted from
many sources, versioned category rubrics, deterministic plus AI-judged scores, rankings, price and
availability history, and affiliate destinations. A consumer website renders that database as a
searchable, comparable, ranked catalog with evidence-backed editorial summaries. An admin "mission
control" lets one operator watch automated pipelines produce drafts, approve or reject them, see costs,
and roll back changes. The pipeline is a lifecycle state machine (discovered through published and
monitored) with weekly incremental refresh.

The defensible asset is the longitudinal, provenance-rich, normalized evidence corpus and the versioned
scoring methodology on top of it. Any single page can be copied; the accumulated cross-source, time-series
data and the ability to re-score everything under a new rubric version cannot be reproduced cheaply.

How the two revenue streams complement each other: affiliate revenue is funded by anonymous search
traffic and pays per transaction, so it rewards breadth (many categories, many products) and freshness
(prices, availability). Subscription revenue is funded by identified users and pays for depth
(personalized weighting, alerts, history, the assistant), so it rewards the same underlying data but
monetizes the people whose intent is strongest. The free site is the acquisition funnel for both; the
database is the shared cost. A price-alert subscriber who buys through your link produces both revenue
types from one relationship.

---

## STEP 2 — Critique

Ordered by how much each could change the design.

1. Data-source legality is the largest risk and it lands on the first source you named.
   - Amazon customer reviews are not available in the Product Advertising API and the Associates
     Operating Agreement prohibits scraping Amazon. Storing Amazon review text is not a grey area.
     Prices from PA-API may only be displayed if refreshed within 24 hours; PA-API access itself is
     revoked without qualifying sales. Amazon can supply identifiers, images, titles, and current price
     under those rules, and nothing else.
   - Reddit's Data API terms restrict commercial use and require a licensing agreement for it; scraping
     is actively blocked; content must be deleted when the author deletes it.
   - YouTube Data API provides metadata and comments within a daily quota; transcripts are not an
     official endpoint.
   - X API pricing makes it poor value for appliance research. TikTok has no viable API for this.
   - Professional reviews are copyrighted; you may store scores, verdict summaries, and links, not text.
   - Implication: the schema must separate raw third-party content (short excerpts, URL, retention
     rules, deletable) from derived observations (yours, permanent). Most value must sit in derived
     observations. The Step 5 proposal will treat "which sources are legally storable" as a first-class
     design input, and the MVP should assume Amazon supplies identity and price only.

2. Washing machines is a strong pipeline test category and a weak Amazon affiliate category.
   Large appliances are mostly bought at Best Buy, Home Depot, Lowe's, and manufacturer sites, and
   appliance commission rates across networks are low (roughly one to three percent). A $1,000 washer
   can yield around $10 to $30. Amazon also closes Associates accounts without three qualifying sales in
   the first 180 days. Recommendation: keep washers as the pipeline proof, and add a second category
   chosen for affiliate validation (high Amazon share, high unit volume, moderate price: headphones, robot
   vacuums, espresso machines, air purifiers). Join Impact/CJ/Rakuten for Best Buy and Home Depot early.

3. Subscription economics for durable goods research are structurally hard.
   A person buys a washer once a decade. Recurring billing needs recurring need. Comparable paid
   services (Consumer Reports, RTINGS, Wirecutter via NYT) survive on brand, lab testing, or bundling,
   and their price points sit in the low single digits to about ten dollars a month. Three honest
   responses: (a) breadth solves it only after many categories exist, so the paywall should launch
   after traffic exists, not with it; (b) alerts and monitoring are the only feature that creates a
   reason to stay subscribed between purchases; (c) the database may monetize better through B2B
   (data API, brand insights, retailer feeds) than through consumers, and the architecture should not
   preclude that. I will propose a paid test, but treat willingness to pay as unproven.

4. "One new category per day, as many products as practical" is an uncapped cost function.
   Washing machines alone have several hundred current US models. At realistic collection plus AI
   costs per product, an uncapped daily category could cost hundreds to over a thousand dollars per
   day. The system needs a category budget (max products, max sources per product, max AI spend) as
   a configuration, and the daily category cadence should be budget-driven, not time-driven.

5. Scoring risk: criteria like "cleaning performance" and "reliability" cannot be derived from specs.
   They come from lab tests you do not run and from owner reports that are noisy and biased. Without
   evidence-sufficiency gating (a criterion is unscored below N qualifying observations and the overall
   score shows its confidence), the AI will produce plausible-sounding fiction. Objective scoring also
   needs per-category normalization functions (min-max, log, thresholds) defined in the rubric, and
   the rubric must record which criteria are objective, which are subjective, and the evidence floor.

6. SEO risk is higher than the brief assumes.
   Google's scaled-content and site-reputation policies specifically target programmatic AI pages, and
   its product review guidance rewards first-hand testing evidence. AI Overviews have cut clicks to
   review sites materially. A thousand thin category pages is the worst-case profile. Mitigations:
   publish fewer, deeper pages; expose unique data (aggregated sentiment counts, price history,
   provenance) that AI answer engines can cite; structured data everywhere; build a direct channel
   (email list from alerts) so you are not solely dependent on Google.

7. Automation-platform risk: n8n is the right visual orchestrator but a poor home for logic.
   Large n8n workflows become unreadable, error handling is per-workflow, and version control is
   JSON export. The design should keep n8n as a thin scheduler/orchestrator and push deterministic
   logic into Postgres functions and small edge functions with tests. Alternatively skip n8n entirely
   and use Supabase-native scheduling and queues with AI-generated code. This is a real fork; see Step 4.

8. Entity resolution will consume more operator attention than any other step.
   Model-number variants (LG WM4000HWA vs WM4000HBA are colors of one product; suffixes differ by
   retailer) require blocking rules, identifier matching (GTIN/UPC where obtainable), AI adjudication,
   and a human merge/split queue. Budget admin UI for this from day one.

9. AI reliability: extraction and classification should use structured outputs with schema validation,
   independent QA passes, and evals stored in the repo. Editorial text must be generated only from
   stored observations with citations back to observation IDs, and regenerated when observations change.

10. One-person operation: every vendor added is another dashboard, invoice, and failure mode.
    The stack in Step 4 aims for five paid vendors at MVP. Anything beyond that needs a stated reason.

11. Payment and tax: selling subscriptions internationally as a sole operator triggers VAT/GST
    registration obligations in many jurisdictions. A Merchant of Record removes that. The premium
    is roughly two to three percentage points over a plain processor; for a one-person business
    with modest volume that is usually worth it, but it depends on your entity and launch markets.

12. Scale risk is low if the data model is right. Postgres handles millions of observations on a
    single Supabase instance; the transitions are about partitioning, search infrastructure, and moving
    hot paths out of n8n, not about replatforming.

---

## STEP 3 — Clarifying questions (highest leverage first)

A. Business and legal
1. What legal entity and country will operate this, and will you sell subscriptions outside your home
   country at launch? This decides Merchant of Record versus plain processor.
2. What monthly budget are you willing to run at during MVP for all tooling, data, and AI combined?
   Rough bands: under $300, $300 to $1,000, $1,000 to $3,000, more.
3. How many hours per week can you give operations once it is running, and what is your target date
   for a working washing-machine pipeline?
4. Do you already have an Amazon Associates account, and any accounts with Impact, CJ, Rakuten, or
   Skimlinks? Do you own a domain and brand yet?

B. Technical operating model
5. Have you used n8n, Make, or Zapier before? Have you used Supabase or any Postgres tool?
6. Will you accept a small Next.js codebase that AI maintains and you never hand-edit, or do you
   require a visual builder for the consumer site even at the cost of SEO scale? (My lean is below.)
7. n8n Cloud (paid, zero ops) or self-hosted n8n on a managed host (cheaper, one more server)?
8. Is there any platform you already pay for or want to avoid?

C. Data sources
9. Which of these sources are must-have for the washing-machine MVP versus later: manufacturer specs,
   Amazon identity/price, YouTube reviews, Reddit, professional review sites, retailer reviews
   (Best Buy, Home Depot, Lowe's)?
10. Are you willing to pay for managed data access (Apify or Firecrawl for collection, Keepa for
    Amazon price history, an official Reddit data license if required), and roughly how much per month?
11. Do you want the system to store raw third-party text at all, or only short excerpts plus derived
    observations? My recommendation is the latter; confirm you accept the loss of full-text re-analysis.
12. US market only at launch?

D. MVP scope
13. For washing machines, is a target of roughly 40 to 80 well-covered products acceptable for the
    first pass, rather than every model on the market?
14. Should a second category be chosen specifically to test affiliate revenue (see critique 2)?
    If yes, any preference?
15. Admin approval per product or per batch? At scale, per-product approval of hundreds of drafts
    per category will not hold; I propose per-product with bulk actions and a QA score threshold.
16. Where do you want alerts about failures delivered: email, Discord, Slack, SMS?

E. Monetization
17. Do you accept sequencing the paywall after the free site has measurable traffic, so the first
    paid test runs against real visitors rather than zero?
18. Which single premium feature do you want as the first willingness-to-pay test? My recommendation:
    personalized weighted rankings plus saved comparisons (zero marginal AI cost, demonstrates the
    database). Second: price and ranking alerts (creates retention). The AI assistant is the most
    appealing and the most expensive; I would hold it for a usage-capped tier later.
19. Are you open to non-subscription paid tests too, such as a one-time "buying report" or a
    B2B data API, if consumer subscription conversion is weak?
20. Founding-member annual pricing at launch instead of a free trial: acceptable?

F. AI and accounts
21. Do you have API accounts (not just chat subscriptions) for Anthropic, OpenAI, and xAI? Are you
    comfortable routing through a gateway (OpenRouter, Vercel AI Gateway, or a self-hosted LiteLLM)
    that holds those keys, in exchange for one integration and per-task model switching?
22. What hard monthly AI spend cap should the system enforce?

G. Authentication
23. Is passwordless email (magic link/OTP) plus Google sign-in sufficient at launch?

---

## STEP 4 — Decisions explained, with current leans

1. Database and backend
   - Options: Supabase (Postgres, auth, storage, edge functions, cron, queues, vector); Xano (no-code
     backend, proprietary); Airtable/Baserow (no-code tables, poor at millions of rows and relations);
     Neon plus separate services.
   - Lean: Supabase. It is plain Postgres, so the data survives every other replacement. Low-code with
     a visual table editor and SQL. Lock-in is low (pg_dump). Replacement at scale: managed Postgres
     (RDS/Neon) with the same schema. Search: Postgres full-text first; Typesense or Meilisearch later.
2. Workflow orchestration
   - Option A: n8n as orchestrator, logic in Postgres functions and edge functions. Visual, debuggable,
     matches your preference. Risk: sprawl.
   - Option B: Supabase-native (pg_cron, pgmq queues, edge functions) with AI-generated code and no n8n.
     Fewer vendors, more robust, less visual.
   - Lean: A for MVP with strict rules (one workflow per lifecycle stage, all state in the database, n8n
     never holds business logic), with B as the growth-stage migration path for hot workflows.
3. Consumer website
   - Options: Webflow (visual, CMS item limits and cost at scale), WeWeb/Softr (visual, weaker SEO at
     scale), Next.js on Vercel generated and maintained by AI (code, but you never edit it).
   - Lean: Next.js on Vercel. This is the one place I recommend code, because SEO at 100k pages,
     structured data, and incremental static regeneration are not achievable in visual builders at
     reasonable cost. Operator burden: deploy is a git push; you do not touch code.
4. Admin dashboard
   - Options: Retool/Appsmith/Budibase (low-code internal tools over Postgres), or an admin area in the
     same Next.js app.
   - Lean: admin inside the Next.js app, generated by AI, because it shares auth, data access, and
     deployment with the site. Fallback: Supabase Studio for raw tables, Retool if you prefer drag-drop.
5. Collection layer
   - Options: Apify (marketplace of maintained scrapers, per-run cost), Firecrawl (crawl/scrape to
     markdown, good for manufacturer sites), official APIs (YouTube, Amazon PA-API), custom scrapers
     (avoid).
   - Lean: official APIs where they exist, Firecrawl for manufacturer and retailer spec pages, Apify
     for anything needing maintained scrapers. Every adapter writes to the same raw-source tables.
6. AI provider abstraction
   - Lean: a gateway from day one (OpenRouter or Vercel AI Gateway), because it costs nothing to adopt
     and gives per-task model routing and spend tracking. Do not build your own abstraction layer.
     Structured outputs and evals live in the repo, independent of provider.
7. Payments
   - Stripe (processor, lowest fees, best tooling, you handle tax via Stripe Tax and registrations);
     Stripe Managed Payments (Stripe as Merchant of Record, newer); Paddle (Merchant of Record, mature,
     around five percent plus a fixed fee); Lemon Squeezy (Merchant of Record, now owned by Stripe);
     Polar (Merchant of Record for developers, built on Stripe).
   - Lean: a Merchant of Record for a one-person business selling internationally, with Paddle as the
     first choice pending fee verification, and Stripe as second choice if you sell only domestically.
     Entitlements live in our database either way. Full comparison table comes in Step 5 after
     deep-research verifies current fees and terms.
8. Authentication
   - Lean: Supabase Auth (magic link, OTP, Google). Clerk only if you want polished prebuilt UI and
     accept another vendor. Auth0 is overkill.
9. Site structure
   - Lean: one domain, directory structure by category family, as you prefer. Subdomains split
     authority; separate domains multiply operations. Revisit only if a category family becomes a
     distinct brand.
10. Email and alerts
   - Lean: Resend or Postmark for transactional email (alerts, magic links). Klaviyo, already connected,
     is for marketing and can be added later.

Estimated MVP vendor count: Supabase, Vercel, n8n, one collection provider, one AI gateway, one email
provider, one payment provider. Seven, with payments deferred to Phase 2.

# Decision: RateMyBed, a review platform with analytics and comparisons

Status: decided by the operator, 2026-09-29. Supersedes the "editorial database first" framing in
`09-sleep-niche-plan.md` where they differ. Domain: ratemybed.com (and .ai), unresolved as of the DNS check;
operator reports no existing trademark.

## The model
1. Center: a public database of verified owner ratings of beds and sleep products. Each entry records the
   make-up of the bed (brand, model, size, type, firmness, layers if known), purchase date, price paid,
   retailer, sleeper position, body weight band, criterion star ratings, likes, dislikes, free text, and a
   photo. Every review is public and indexable, published regardless of rating.
2. The unlock: submitting one verified review (or subscribing) unlocks the analytics layer: filters by body
   type and sleeper position, durability by year, complaint frequency, price history, unlimited comparisons,
   the full quiz, alerts. The reviews themselves are never gated.
3. Comparisons and rankings: model-versus-model and brand hubs built from owner data plus objective spec and
   warranty data; "top rated" lists computed with Bayesian shrinkage so few-review models cannot dominate.
4. Revenue: affiliate links on top-rated items and comparisons (primary); premium access to advanced
   analytics and sentiment summaries; later B2B analytics for brands.
5. Seeding: owner panel surveys run with explicit consent to publish anonymized responses as "owner
   reports", visually distinct from site reviews, so every top model has structured entries before indexing.
6. Operated as a review platform: verification, moderation, a published review policy, incentive
   disclosure on each review, brand response and dispute processes, no sentiment-conditioned incentives, no
   suppression of negatives.

## What changes in the design documents
- Conventions (`CLAUDE.md`): native reviews move to the first value pillar; reviews are public; the gate
  sits on analytics; seeding and indexing rules are stated.
- Schema (`03-software-proposal.md` section 11): add `reviews`, `review_updates` (follow-ups at 6, 12, 24
  months), `review_verifications`, `moderation_events`, `brand_responses`, `disputes`, `consents`; survey
  responses gain a `publishable` flag; `entitlements` gains an `unlocked_by_review` source.
- Admin: moderation queue, verification queue, brand response inbox, review-fraud signals.
- SEO: Review and AggregateRating from user-sourced reviews only; review policy page; model pages indexed
  only when they hold at least five verified entries (reviews or owner reports) plus spec and warranty data.
- Growth: review follow-up flows ("how is it holding up" at 6, 12, 24 months) become the retention engine
  and the durability dataset.

## Pre-development checklist (what remains before Phase 0)

### A. Brand and legal foundation
1. Register ratemybed.com and ratemybed.ai; claim handles on YouTube, TikTok, Instagram, Pinterest, X.
2. File a trademark application for RateMyBed (word mark) once the name is final.
3. Draft Terms of Service (user content license, acceptable use, Section 230 posture, Consumer Review
   Fairness Act compliance), Privacy Policy (CCPA and similar), Review Policy (collection, verification,
   moderation, incentive disclosure, brand responses, disputes), Affiliate Disclosure, Cookie notice.
4. Register a DMCA agent with the US Copyright Office (required for safe harbor on user content).
5. Decide the business entity question (sole proprietorship versus LLC) before user data and revenue exist.

### B. Review platform design (decisions that shape the schema)
6. Review form fields and which are required; the minimum for an entry to count as complete.
7. Verification tiers: photo only; photo plus receipt or serial; and what each tier displays.
8. Moderation rules: AI pre-screen criteria, human review triggers, prohibited content, appeal path.
9. Aggregation math: criterion averages, Bayesian prior and minimum count, how owner reports and site
   reviews are weighted relative to each other and to objective data.
10. Anti-gaming: rate limits, duplicate detection, brand-affiliated reviewer handling, burst detection.
11. Unlock rules: what one review unlocks, for how long, whether a follow-up review renews it, what
    subscription adds beyond the unlock.
12. Follow-up cadence and incentives for updates at 6, 12, and 24 months.
13. Brand participation: response rights, verified-brand badges, no paid placement in rankings.

### C. Seeding plan
14. Survey design with consent language for publication; sample size per model (target 15 to 30 entries
    for the top 40 models); Prolific budget (roughly $1,500 to $3,000 for the first pass at $0.60 to $0.90
    per complete plus screen-outs).
15. Founding-reviewer campaign: personal network, sleep and bedding communities, incentive for the first
    500 verified reviews.
16. Indexing threshold and the batch-one page list.

### D. Product and data scope
17. Analytics layer definition: the exact charts, filters, and summaries behind the unlock.
18. Comparison tool specification: fields compared, how owner data and objective data are shown side by
    side, how affiliate offers appear.
19. Sentiment summary rules: paraphrased, criterion-level, source counts shown, generated only from stored
    reviews and observations.
20. Rubric v1.0 for mattresses, blending owner criteria with objective data; evidence floors.
21. Raw content policy confirmed: excerpts plus derived observations only for third-party sources.

### E. Operations
22. Affiliate network applications once the site skeleton and policies are live (Impact, Awin, CJ,
    FlexOffers, Sovrn; Amazon later).
23. Email: Resend for transactional, Klaviyo for marketing; flows for verification, unlock, follow-ups,
    price alerts.
24. n8n hosting choice (Docker locally during build or Railway at about $5 per month).
25. Survey panel confirmed (Prolific via API).
26. Operator hours per week and target date for batch one.
27. Approval to start Step 0: enable the Supabase plugin against a new dev project; create the conventions
    skill after schema approval.

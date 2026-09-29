# Project conventions (read first in every session)

This repository is the design and, later, the implementation of RateMyBed (ratemybed.com): a review platform
for beds and sleep products where verified owner ratings are the center, with analytics, comparisons, and
rankings built on top. See `docs/11-ratemybed-platform-decision.md`. Design documents live in `docs/`. Nothing has been built yet as of
2026-09-29; Phase 0 starts only with the operator's explicit approval.

## Build principle: earn trust, add real value

Every workflow, page template, schema decision, and prompt must serve one goal: the site is BUILT so that
Google can trust it and so that visitors get value they cannot get elsewhere. The value lives in five things,
and anything that does not strengthen one of them is suspect:

1. Native reviews: verified owner reviews collected on this site, always public and indexable, published
   regardless of rating, with the incentive disclosed on each review. Submitting one review unlocks the
   analytics layer; the reviews themselves are never gated.
2. Native scoring: our own versioned rubric blending owner ratings (Bayesian-shrunk, minimum counts) with
   objective spec and warranty data; deterministic where objective, evidence-floored where subjective.
3. Comparisons: model-versus-model and brand-versus-brand with real deltas (specs, warranty terms, price
   history, owner data), not reshuffled listicles.
4. Review summaries: paraphrased, criterion-level sentiment summaries grounded in stored reviews and
   observations, each with visible sources and counts.
5. Suggestions: personalized rankings and the quiz, driven by the rubric and the user's stated needs, with
   the reasoning shown.

Rules that follow from the principle:
- One index rule, implemented only by `index_gate` (master-plan.md section 8.2) and stored only in
  `pages.indexable`: a model page is indexable when (a) the data card is complete (every required attribute
  canonical, warranty years and trial nights parsed, a price point within 30 days), (b) an owned signal
  exists (30 or more days of first-party price history, or 3 or more published native reviews), (c) evidence
  exists (20 or more live observations across 2 or more sources, or 5 or more published native reviews), and
  (d) the unique-content ratio is 0.5 or higher. Everything else is served with noindex. Never index empty
  pages. Expand the index in evidence-driven batches; there is no URL-per-month target.
- Gated content is never server-rendered into cached pages: indexable routes render the visitor view only,
  gated fragments are client-fetched through authenticated route handlers, and CI fails on any gated marker
  in an anonymous fetch.
- Run it as a review platform: verification, moderation, a published review policy, brand responses,
  disputes, no sentiment-conditioned incentives, no suppression of negatives, DMCA agent registered.
- Every generated sentence must trace to a stored fact or observation with provenance. No claim without a
  source row.
- Third-party reviews (collected via managed providers) feed the review-intelligence layer only: paraphrased
  observations, excerpts of at most 25 words, source pointers, raw text purged within 30 days. They are
  never inserted into native reviews, never counted in AggregateRating, never republished in full. See
  `docs/12-seeding-decision.md`. Amazon collection and an Amazon Associates account are mutually exclusive.
- AggregateRating only from published native reviews (unweighted mean and count, excluding
  material-connection reviews, shown at 5 or more). Editorial scores go in Review with the operator as the
  named Person author and the site as publisher; pros and cons only from criteria with native evidence.
- YouTube-derived text (titles, comments, author names) is never kept past 30 days; only ids and counts
  persist. Receipts and law labels are deleted 30 days after verification. Photos with people are rejected.
- Real byline (the operator), a methodology page, an AI-assistance disclosure on generated sections, and
  affiliate disclosure adjacent to links with rel="sponsored".
- No moderation or dispute action keys on rating value; dispute grounds are enumerated; brand responses are
  plain text, moderated before publication; brand badges are free with optional nofollow links, never paid
  or required.
- No Reddit scraping (links only), no Gemini grounding for stored data, no bought or required links, no aged
  domain, no paid placement in rankings.
- Source of truth: `master-plan.md` at the repository root. Where a numbered document in `docs/` differs,
  the master plan wins.

## Operating constraints
- One operator, no-code or low-code first, zero spend until unavoidable, everything in Postgres with
  provenance, every material change logged and reversible.
- Stack decisions and their rationale: `docs/03-software-proposal.md`. Evidence and legal sourcing:
  `docs/04-evidence-strategy.md`, `docs/06-niche-and-attribution.md`. Niche plan and growth:
  `docs/09-sleep-niche-plan.md`, `docs/10-seo-and-growth-plan.md`. Research appendices: `docs/research/`.

## Session hygiene
- Discussion documents are numbered in `docs/`; add the next number rather than editing history.
- Commit and push to the designated branch after each document.

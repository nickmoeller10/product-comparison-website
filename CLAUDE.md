# Project conventions (read first in every session)

This repository is the design and, later, the implementation of a product intelligence platform focused on
the sleep, mattress, and bedding niche. Design documents live in `docs/`. Nothing has been built yet as of
2026-09-29; Phase 0 starts only with the operator's explicit approval.

## Build principle: earn trust, add real value

Every workflow, page template, schema decision, and prompt must serve one goal: the site is BUILT so that
Google can trust it and so that visitors get value they cannot get elsewhere. The value lives in five things,
and anything that does not strengthen one of them is suspect:

1. Comparisons: model-versus-model and brand-versus-brand with real deltas (specs, warranty terms, price
   history, owner data), not reshuffled listicles.
2. Review summaries: paraphrased, criterion-level summaries grounded in stored observations, each with
   visible, clickable sources.
3. Suggestions: personalized rankings and the quiz, driven by the rubric and the user's stated needs, with
   the reasoning shown.
4. Native reviews: verified owner reviews collected on this site, published regardless of rating, with the
   incentive disclosed on each review.
5. Native scoring: our own versioned rubric, deterministic where the data is objective, evidence-floored
   where it is subjective, reproducible from the database.

Rules that follow from the principle:
- A page is indexable only when it passes the completeness gate (spec coverage, evidence floor, source
  count, unique-content ratio). Everything else is served with noindex.
- Every generated sentence must trace to a stored fact or observation with provenance. No claim without a
  source row.
- Never store or republish third-party review text; store paraphrased observations plus URL pointers.
- Never mark up AggregateRating from third-party ratings. Editorial scores go in Review with a named author.
- Real byline (the operator), a methodology page, an AI-assistance disclosure on generated sections, and
  affiliate disclosure adjacent to links with rel="sponsored".
- Expand the index in measured batches driven by Search Console evidence, never by page-generation capacity.
- No scraped Reddit or Amazon reviews, no Gemini grounding for stored data, no bought links, no aged domain.

## Operating constraints
- One operator, no-code or low-code first, zero spend until unavoidable, everything in Postgres with
  provenance, every material change logged and reversible.
- Stack decisions and their rationale: `docs/03-software-proposal.md`. Evidence and legal sourcing:
  `docs/04-evidence-strategy.md`, `docs/06-niche-and-attribution.md`. Niche plan and growth:
  `docs/09-sleep-niche-plan.md`, `docs/10-seo-and-growth-plan.md`. Research appendices: `docs/research/`.

## Session hygiene
- Discussion documents are numbered in `docs/`; add the next number rather than editing history.
- Commit and push to the designated branch after each document.

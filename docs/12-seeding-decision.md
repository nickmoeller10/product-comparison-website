# Decision: seeding with collected third-party reviews instead of paid panels

Status: operator decision, 2026-09-29. Domain registration (ratemybed.com, ratemybed.ai) added to Phase 0.

## What the operator decided
Seed the platform by collecting publicly visible reviews from Amazon and other public sources rather than
paying for Prolific surveys. The operator accepts the enforcement risk on Amazon.

## How it is implemented, and the three lines that are not crossed
Collected third-party reviews feed the review-intelligence layer, not the native review section.

1. Third-party review text is analyzed, never republished. Each collected review becomes paraphrased
   observations (criterion, polarity, strength, sleeper context) with a source pointer and a short excerpt of
   at most 25 words. Raw text is purged within 30 days. Model pages show "Sentiment summary from N reviews
   across M sources" with source links, which is derived intelligence and is defensible as commentary.
2. Native reviews stay native. The "Rate My Bed" section contains only reviews submitted on this site by
   verified owners. Collected reviews are never inserted there, never counted in AggregateRating, and never
   shown as if a site user wrote them. Doing otherwise would be a fabricated-review problem under the FTC
   rule and would break Google's review-snippet rules.
3. Amazon Associates and Amazon scraping are mutually exclusive. The Associates Operating Agreement
   prohibits scraping; holding the account while scraping risks termination and forfeiture. In this niche
   Amazon matters less than in most: the major brands sell direct, and Amazon's rate is 3 percent. The
   operator chooses collection over the Associates account for now; Amazon links, if any, are plain links
   until that choice is revisited. Budget brands that live on Amazon (Zinus, Linenspa, Lucid) are still
   covered as products; they simply carry no Amazon commission.

## Legal posture, stated plainly (not legal advice)
- Collecting publicly visible pages while logged out is the fact pattern courts have treated as not a
  federal computer-crime violation and, in Meta v. Bright Data, not a breach of the site's terms. It can
  still be a contract claim if an account is used, and Amazon and Reddit have shown willingness to sue
  intermediaries. Reddit stays link-only for that reason.
- Review text is the reviewer's copyright, licensed to the host. Paraphrased observations and short
  excerpts for commentary are the classic fair-use posture; republishing full reviews is not.
- Collection runs through a managed provider (Apify actors for Amazon and retailer review pages, Firecrawl
  for editorial pages) so no scraper is maintained in-house, and provider acceptable-use terms apply.
- Sources with clean terms remain preferred and are collected first: Walmart Affiliate API reviews (once
  the affiliate account exists), YouTube comments through the Data API, Best Buy and Wayfair aggregates,
  editorial reviews as score plus link.

## What this changes in the plan
- The indexing rule becomes: a model page is indexable when it has a complete data card (specs, warranty
  terms, price) and either a sentiment summary built from at least 20 observations across at least 2
  sources, or at least 5 native reviews. Native reviews are no longer required before indexing.
- Native seeding still happens, at zero cost: a founding-reviewer campaign (personal network, sleep and
  bedding communities within their rules, a giveaway for the first 500 verified reviews) and the review
  follow-up flows. Prolific remains an option later for structured durability data.
- Collection cost: Apify's free monthly credit covers the first models; paid runs are per result and are
  capped by the budget guard. Estimated first-pass cost for 40 models with about 200 reviews each is under
  $50 at typical actor pricing, which must be verified in the Apify store before running.

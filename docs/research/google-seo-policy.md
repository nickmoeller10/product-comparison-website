# Google policy and SEO landscape for an AI-assisted product comparison site (29 Sep 2026)

Method: official docs were mostly blocked by the sandbox proxy; quotes come from search excerpts of the
official pages and reputable secondary coverage. Verify verbatim text on the linked Google pages.

## 1. Scaled content abuse (spam policy, March 2024, unchanged through 2026)
Source: developers.google.com/search/docs/essentials/spam-policies
- Definition (quoted): "Scaled content abuse is when many pages are generated for the primary purpose of
  manipulating search rankings and not helping users. This abusive practice is typically focused on creating
  large amounts of unoriginal content that provides little to no value to users, no matter how it's created."
- Examples (quoted): "Using generative AI tools or other similar tools to generate many pages without adding
  value for users"; "Scraping feeds, search results, or other content to generate many pages ... where little
  value is provided to users"; "Stitching or combining content from different web pages without adding value";
  "Creating multiple sites with the intent of hiding the scaled nature of the content".
- Test is method-agnostic: scale + unoriginal/low value + primary purpose of ranking manipulation.
- Google's "guidance on generative AI content" doc restates that AI-generated pages without added value may
  violate the policy and asks that AI-assisted metadata be accurate.
- John Mueller (Sep 2026): programmatic SEO "often leads to a site that's either spam, borderline spam, or low
  quality," and Google can lose faith in a whole site because of low-value programmatic sections.

## 2. Site reputation abuse and expired domain abuse
- Site reputation abuse: third-party pages published with little first-party oversight to exploit the host's
  signals. From Aug 30 2026 manual actions no longer affect EEA users (DMA probe); unchanged elsewhere.
  Relevance: low for an owner-published site unless it hosts network/guest content or sells subfolders.
- Expired domain abuse: buying an aged domain to host unrelated content. Do not launch on a bought aged domain
  with unrelated history.

## 3. Reviews guidance and reviews system
Source: developers.google.com/search/docs/specialty/ecommerce/write-high-quality-reviews
- Wants: evaluation from the user's perspective, demonstrated expertise, "evidence such as visuals, audio, or
  other links of your own experience", "quantitative measurements about how something measures up in various
  categories of performance", differentiation from competitors, coverage of comparable items, pros and cons
  "based on original research", product evolution, key decision factors, "links to multiple sellers", and
  ranked lists that explain the ranking rationale.
- The reviews system is still documented as a standalone system (doc updated Dec 2025); updates are
  unannounced. Review-heavy sites see their biggest swings in core and spam updates.

## 4. AI-generated content guidance
- Feb 2023: "appropriate use of AI or automation is not against our guidelines"; automation "with the primary
  purpose of manipulating ranking" is spam. "Who, How, Why" self-assessment: bylines, disclose automation where
  reasonably expected, content primarily for people.
- Helpful-content doc: if automation substantially generates content, consider disclosure and explain how and why.
- "AI features and your website": no additional requirements for AI Overviews or AI Mode; same index.
- May 2026 "Optimizing for generative AI features": valuable, unique, non-commodity content; unique point of
  view; llms.txt has no special treatment; "still SEO".

## 5. 2025-2026 update commentary
- Dec 2025 core update: large publishers' affiliate sections and "self-serving listicles" hit; algorithmic
  rather than manual treatment.
- March 2026 core update: "first-party, official-source correction". "Platforms that aggregate, list, or comment
  on other people's content lost visibility, while sites that created or owned the content gained visibility."
  NerdWallet -15.9%; templated city-swap pages lost.
- June and August 2026 spam updates: case studies of sites with 85% programmatic pages, "thin affiliate pages
  built on scraped product data", wiped out; "scale combined with very little unique value". Recovery takes
  months. A September 2026 spam update began Sep 24.
- Differentiators: proprietary data, first-hand testing, verifiable authorship, closeness to the source.
- Detailed.com (Sep 2025): big media brands hold 86% of #1 content rankings; only 4 of top 100 product-review
  domains are independent; Reddit dominates review SERPs.
- AI Overviews: Ahrefs Dec 2025, position-1 CTR down 58%; AIOs on roughly half of queries; cited pages get
  2 to 5 times the CTR of uncited ones (Seer, 2026).

## 6. Structured data eligibility
- Product snippet: for pages that "publish product reviews, and/or aggregate information from other sites";
  needs name plus one of review, aggregateRating, offers. Pros/cons (positiveNotes/negativeNotes) only on
  editorial review pages; at least two statements, visible on page.
- Review snippet: nested in the reviewed type; author name and ratingValue required; "Don't aggregate reviews
  or ratings from other websites"; AggregateRating "must be sourced directly from users". Editorial score goes
  in Review.reviewRating with an author, never AggregateRating from third-party ratings.
- ItemList carousel for Product: EEA, Turkey, South Africa only (Sep 2026). Harmless in the US, no rich result.
- FAQPage: rich results only for government and health sites since Aug 2023. HowTo removed.
- BreadcrumbList: supported. Organization and Person: recommended for entity trust.

## 7. Thin affiliation (spam policy)
Quoted: "Thin affiliation is the practice of publishing content with product affiliate links where the product
descriptions and reviews are copied directly from the original merchant without any original content or added
value." Counter: "Good affiliate sites add value by offering meaningful content or features", original reviews,
ratings, comparisons, hands-on testing. Use rel="sponsored" on affiliate links.

## 8. Practitioner consensus
- "Do not run a programmatic project without unique data" (Patrick Stox): derived metrics nobody else computed.
- Quality gates before indexing: noindex sparse or duplicate pages; 50 to 60 percent page-specific content;
  publish strongest pages first, expand on indexation and traffic evidence.
- Crawl budget: no URL dumps; clean sitemaps and canonicals.
- E-E-A-T: bylines, author bios, methodology pages, disclosed AI use, evidence, original data. Primary research
  earns about 3.3x more AI citations; fresh content is about 3x likelier to be cited (Kevin Indig).

## Implications (12 rules)
1. Every indexed page carries something not obtainable from a merchant feed: computed scores, normalized spec
   comparisons, price history, curated evidence with citations. Feed data plus AI prose gets noindex.
2. Gate indexation by a data-completeness score; start with a few hundred high-confidence pages at most.
3. Never republish other sites' reviews or ratings; never mark up AggregateRating from third-party ratings.
4. Use Product snippet, Review with named author, pros/cons, BreadcrumbList, Organization, Person.
5. Publish a methodology page and link it from every ranking page; add an AI disclosure.
6. Real bylines and an author page for the owner; no fake editorial team.
7. Link to multiple sellers with rel="sponsored"; show price freshness; disclose affiliation.
8. Boilerplate ratio under about 50 percent; enforce a minimum unique-content threshold per template.
9. First-party control only: no guest or network content, no sold subfolders, no bought aged domain.
10. Plan for AI Overviews: optimize to be cited; build direct, newsletter, and community channels.
11. A large low-value section drags the whole domain; prune or noindex weak categories quickly.
12. Depth in a few categories with original data beats breadth.

# Decisions Log

Recorded from the operator's answers on 2026-09-29. These are inputs to the Step 5 proposal.

| # | Topic | Decision |
|---|---|---|
| D1 | Operator / entity | Sole proprietor, Nicholas Moeller, United States. |
| D2 | Launch market | US only. |
| D3 | Subscriptions at launch | Not sold at launch. Infrastructure (accounts, entitlements, plans) designed from the start; checkout enabled later. |
| D4 | Budget posture | Keep initial spend as low as possible. Do not start paying for a service until there is no free alternative. Prefer free tiers. |
| D5 | AI spend | Anthropic API account exists. Use only what is necessary. Measure AI cost per workflow before scaling any workflow. Hard caps required. |
| D6 | First category | Washing machines. Start with 10 products. Grow product count based on observed pipeline speed and cost. |
| D7 | Category cadence | Target one new category per day, but the pipeline must be configurable and budget-capped. |
| D8 | Second category | Choose one with strong Amazon presence to test Amazon affiliate revenue. |
| D9 | Affiliate programs | Pursue the best options: Amazon, Walmart, Best Buy, Target, Home Depot, Lowe's, plus sub-affiliate networks if they accept a new site. No accounts exist yet. |
| D10 | Legality | Stay fully within platform terms and the law. No Amazon review scraping. Explore legal alternatives. |
| D11 | Must-have sources (MVP) | Reddit, blogs and editorial sites, manufacturer specifications, Google search discovery (find pages discussing a product and verify they actually mention it). |
| D12 | Managed data spend | Willing to pay a little, price-dependent. |
| D13 | Raw content storage | Pending: recommendation is excerpts plus derived observations only. |
| D14 | Orchestrator | n8n. |
| D15 | Entity resolution | Include a merge and split queue in the admin. |
| D16 | Admin approvals | Per-product with bulk actions. |
| D17 | Failure alerts | Ignore for now; eventually Discord. |
| D18 | Paywall timing | Only after the free site has measurable traffic. |
| D19 | First paid test | Personalized weighted rankings plus saved comparisons. Alerts second. AI assistant later, usage-capped. |
| D20 | Recurring value thesis | Value comes from insight into the application, not from repeat purchases of one product: recommendations, email digests, in-depth research access. |
| D21 | One-time purchases | Open to one-time reports (single product with price history, or partial category report with subscription for full access). |
| D22 | Pricing structure | Monthly base price. One-week free trial. |
| D23 | Auth | Passwordless email plus Google sign-in. Prefer a managed auth platform. |
| D24 | Database | Must be queryable manually (SQL, table view) and AI-native (vector search, agent access). |
| D25 | Codebase | AI-maintained codebase accepted. |
| D26 | B2B data API | Recognized as a future revenue stream; architecture should not preclude it. |
| D27 | SEO | Operator asked for a specific strategy addressing Google's scaled-content risk. |
| D28 | Operator experience | No prior use of n8n, Make, Zapier, or Supabase. Design and documentation must assume a first-time user. |
| D29 | Hours per week / target date | Not yet answered (question was unclear; re-asked). |

# Low-cost one-person low-code stack pricing (29 Sep 2026)

Legend: [V] official page fetched; [S] official snippet via search; [3P] third-party roundup, unverified.

## Backend: Supabase
- Free: 500 MB DB, 5 GB egress, 1 GB storage, 50k MAU, 500k edge function invocations, Nano compute, 2
  projects; auto-paused after 7 days inactivity; no automatic backups. [S]
- Pro $25/mo: 8 GB DB, 250 GB egress, 100k MAU, 2M edge invocations, $10 compute credit, no pausing, daily
  backups 7-day retention. Compute add-ons: Micro about $10, Small about $15, Medium about $60. [V]
- pgvector on all plans [S]; pg_cron on Free/Pro [3P]; pgmq Queues and pg_net enabled from Integrations on all
  plans [S]. Free overage makes the project read-only rather than billing.

## Frontend: Vercel
- Hobby $0: 100 GB transfer, 1M edge requests, 1M function invocations, 1M ISR reads, 100 deploys/day.
  Fair use (quoted): "Hobby teams are restricted to non-commercial personal use only. All commercial usage of
  the platform requires either a Pro or Enterprise plan." Commercial = "any deployment tied to financial gain".
  Affiliate links are commercial. [S]
- Pro $20/seat/mo including $20 usage credit; 1 TB transfer; ISR $0.40 per M reads, $4 per M writes. [S]

## Automation: n8n
- Cloud Starter about €24/mo: 2,500 executions, 5 concurrent, unlimited workflows. Pro about €60/mo: 10k
  executions. [3P]
- Self-host: Community edition under the Sustainable Use License (quoted): "You may use or modify the software
  only for your own internal business purposes or for non-commercial or personal use." Running your own site's
  automations is internal business use. Not allowed: hosting n8n for customers or embedding in your product. [V]
- Self-host cost: Hetzner €4 to 8, DigitalOcean $6 to 12, Railway Hobby $5 plus usage, Render from $7; n8n
  wants about 2 GB RAM, so $5 to 12/mo realistic. [3P]
- Features: AI Agent node; Anthropic Chat Model node with prompt caching [V]; OpenRouter and OpenAI nodes [S];
  Supabase and Postgres nodes; Error Trigger node and per-workflow error workflow; queue mode with Redis and
  Postgres workers. Email: SES and Brevo native; Resend and Postmark via HTTP.

## AI providers and gateways (per 1M tokens)
- Anthropic [V]: Haiku 4.5 $1 in / $5 out; Sonnet 5.5 $2 / $10 (cache read $0.20); Opus 5.5 $4 / $20;
  Fable 5.1 $10 / $50. Batch API 50 percent off. Cache read 0.1x (lower on Opus and Fable). Web search $10 per
  1k searches.
- OpenAI [3P]: GPT-5-nano $0.05 / $0.40; GPT-5-mini $0.25 / $2; GPT-5 $1.25 / $10; batch 50 percent off.
- xAI [3P]: Grok 4 Fast $0.20 / $0.50; Grok 4.7 $2 / $6.
- OpenRouter: 5.5 percent fee on credit purchases; BYOK fee-free to $25k/mo; per-key spend limits; usage API. [S]
- Vercel AI Gateway: 0 percent markup, including BYOK; needs a Vercel team. [S]
- LiteLLM: open source self-host, needs VPS and Postgres. [S]

## Web data providers
- Firecrawl: 1,000 free credits/mo; Hobby $19 (5k credits); 1 credit per page, extract formats 5 credits. [3P]
- Apify: $5 free usage/mo; Starter $29; SERP actors $0.05 to 0.35 per 1k results; Reddit actors $0.60 to 2 per
  1k posts (not usable here for legal reasons); Amazon actors (not usable). [S/3P]
- Serper.dev: 2,500 free queries; then $0.30 to $1 per 1k. [3P]
- SerpAPI: 100 free/mo; $25/mo for 1k. [3P]
- Brave Search API: $5 free credit/mo; $5 per 1k. [3P]
- Google Custom Search JSON API: closed to new customers; ends Jan 1 2027. Do not build on it. [S]

## Transactional email
- Resend 3,000/mo free (100/day); Postmark 100/mo free; Brevo 300/day free with branding, native n8n node;
  Amazon SES no free tier for accounts created after July 2025. [S/3P]

## Payments
| Vendor | Fee | MoR | Notes |
|---|---|---|---|
| Stripe | 2.9% + 30c; Billing +0.7%; Tax +0.5% | No | Checkout, Customer Portal, webhooks, trials, coupons, Payment Links; instant onboarding [S] |
| Stripe Managed Payments | about 6.4% + 30c (7.1% with Billing) | Yes, 75+ countries | Eligibility review; public preview since Feb 2026 [S] |
| Paddle | 5% + 50c | Yes | 3 to 7 day verification, manual review, rejections for entity mismatch or missing policy pages [3P/S] |
| Lemon Squeezy | 5% + 50c | Yes | Acquired by Stripe; signups reportedly gated mid-2026; migrating users toward Managed Payments [S] |
| Polar.sh | 5% + 50c (orgs created after May 27 2026); Pro $20/mo lowers to 3.8% + 40c | Yes | Instant global onboarding; checkout, portal, trials, discounts, webhooks [S] |
| Gumroad | 10% + 50c plus card fees | Yes | Weak SaaS portal [3P] |

## Auth
- Supabase Auth: 50k MAU Free, 100k Pro, then $0.00325/MAU. [S]
- Clerk: 50,000 monthly retained users free (since Feb 2026); Pro $25/mo. [S]
- Auth0: 25k MAU free; Essentials $35/mo. [3P]

## Monitoring and search
- Sentry Developer: free, 1 user, 5k errors/mo. Better Stack: 10 monitors free. UptimeRobot free is
  non-commercial only. [3P]
- Algolia 10k records and 10k searches free; Meilisearch Cloud from $20; Typesense Cloud from about $22;
  Postgres FTS and pgvector $0. [3P]

## Keepa
- API: Starter €49/mo (20 tokens/min); no free tier. Public display terms unverified; assume redistribution of
  raw price history requires confirmation with Keepa. [S/3P]

## Zero-to-minimal spend MVP
- $0 build phase: Vercel Hobby (non-commercial), Supabase Free (pauses after 7 idle days, no backups), n8n in
  Docker locally or Railway $5, Anthropic Haiku 4.5 direct with batch and caching, Serper 2,500 free queries,
  Firecrawl 1,000 credits, Brave $5 credit, Brevo or Resend free, Sentry and Better Stack free, Supabase Auth,
  Stripe account at $0 until first sale.
- First unavoidable costs: affiliate links live means Vercel Pro $20; real users or scheduled scrapers means
  Supabase Pro $25; n8n in production means a $5 to 12 VPS or n8n Cloud €24; Amazon price history means Keepa
  €49; search beyond free credits means Serper at about $1 per 1k.
- Realistic floor once live: about $56 to $71/mo plus AI spend, plus €49 if Keepa is required.

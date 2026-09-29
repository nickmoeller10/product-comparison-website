# Affiliate programs and consumer subscription pricing (29 Sep 2026)

Method caveat: official program pages were blocked by the sandbox proxy; figures come from third-party guides
and trade press. Confirm every number on the official page before relying on it.

## Part A: affiliate programs (US)

### Amazon Associates
- Application: live site with original content and a privacy policy. Conditional approval, then 3 qualifying
  sales within 180 days or the account closes (re-tightened April 14 2026).
- April 14 2026 Operating Agreement changes: items must ship within 180 days to qualify; paid-ad traffic
  disqualified; every page linking to Amazon must carry original commentary, analysis or transformation;
  onsite commissions pay only on the promoted ASIN; offsite Special Links reportedly still earn on the whole
  qualifying cart [verify].
- Spring 2026 rate cuts: trade press reports unannounced cuts of up to 50 percent in some categories, removal of
  milestone bonuses, reduced reporting. No before/after table obtained.
- Category rates (pre-cut baseline, verify current): Kitchen 4.5%; Home, Home Improvement, Furniture, Headphones,
  Lawn & Garden, Pets 3%; PCs 2.5%; TVs 2%; "all other" 4%. No separate Major Appliances line; washers likely
  fall under Home/Home Improvement (about 3%) [verify]. The "8% appliances" figure seen online is Amazon's seller
  referral fee, not the affiliate rate.
- Cookie: 24 hours; cart additions within 24h commissionable if bought within 90 days.
- Disclosure wording: "As an Amazon Associate I earn from qualifying purchases", visible on every page with links.
- Link display: no cloaking that hides the referring site; prices and images from the API only.
- API: PA-API retired May 2026; Creators API requires about 10 qualifying sales in the prior 30 days [verify].
  A new site cannot show live Amazon prices or images on day one; plan for text links until the threshold is met.
- OneLink (geo-redirect) remains free.

### Retailers (Impact)
- Walmart: about 24h approval; 1 to 4 percent by category (home/electronics reported 4%); 3-day cookie; $10 payout.
- Best Buy: 0.5 percent historically with 1-day cookie; rates reportedly cut to 0 percent in 2025; directories
  still show 0.4 to 0.5 percent. Assume 0 to 0.5 percent.
- Target Partners: Home 5 to 8 percent, Electronics 0 percent, 7-day cookie; few large appliances.
- Home Depot: 1 percent on appliances, 24-hour cookie.
- Lowe's: about 2 percent; Creator program up to 20 percent (social-oriented).

### Manufacturer-direct
- LG (CJ): 6 percent electronics, up to 12 percent appliances, 14-day cookie [verify]. Best appliance rate found.
- Samsung: FlexOffers/Awin, about 1.6 to 2.7 percent. GE Appliances: official affiliate page exists.
- Whirlpool about 2 percent; Maytag 1 to 2 percent, 30-day cookie; Bosch and Miele about 3 percent via
  sub-networks; Speed Queen none found. Specialty retailers (Abt, AJ Madison, Appliances Connection) 3 to 6 percent.

### Aggregators and networks
- Skimlinks: 25 percent of commissions; manual approval; denials for "lack of content" common for new sites.
- Sovrn Commerce: about 24h approval; must generate some clicks; fee about 25 percent [verify]. Best day-one path.
- Impact marketplace: reviewed in 72h; declines for low traffic common; each brand approves separately.
- CJ: no traffic minimum but advertisers reject new sites. ShareASale merged into Awin (2025).

### FTC
- 16 CFR Part 255 (revised 2023): affiliate links are a material connection, disclosed clearly and conspicuously,
  near the endorsement, not only in a footer.
- 16 CFR Part 465 (Consumer Reviews and Testimonials Rule, effective Oct 21 2024): bans fake or AI-fabricated
  reviews and company-controlled "independent" review sites; penalties up to $51,744 per violation. AI-synthesized
  text must never be presented as consumer reviews.

### Conversion benchmarks
No appliance-comparison case study found. General affiliate conversion 0.5 to 1 percent; Amazon click-to-order
often cited 5 to 10 percent (anecdotal). Rough washer model: 1,000 visitors x 30% CTR x 3% CR x $900 AOV x 1 to
2 percent = $80 to $160 RPM ceiling, realistically far lower given 24h cookies and in-store purchase leakage.

| Program | Network | New-site acceptance | Appliance rate | Amazon-strength categories | Cookie |
|---|---|---|---|---|---|
| Amazon Associates | Direct | Conditional; 3 sales/180d | about 3 to 4% [verify] | Headphones 3%, Kitchen 4.5%, Home 3% (pre-cut) | 24h |
| Walmart | Impact | 24h | 1 to 4% | about 4% | 3d |
| Best Buy | Impact | Selective | 0 to 0.5% | 0 to 0.5% | 1d |
| Target | Impact | Selective | Home 5 to 8%; electronics 0% | 0% | 7d |
| Home Depot | Impact | Selective | 1% | n/a | 24h |
| Lowe's | Impact | Selective | about 2% | n/a | [verify] |
| LG | CJ | Selective | up to 12% | 6% | 14d |
| Sovrn Commerce | Sub-affiliate | 24h | retailer rate less about 25% | same | merchant's |
| Skimlinks | Sub-affiliate | Often rejects new sites | retailer rate x 75% | same | merchant's |

## Part B: comparable subscription pricing
| Service | Monthly | Annual | Gated |
|---|---|---|---|
| Consumer Reports | $10 Digital | $39 Digital; $64 All Access | Ratings, recommendations, reliability data; about 6M paying members |
| RTINGS | about $10 [verify] | about $45 [verify] | Since Mar 31 2026 full test results members-only |
| Wirecutter (NYT) | $5 per 4 weeks | $40 | Metered paywall |
| Which? (UK) | £8.99 to £11.99 | £89 to £109 | All reviews and Best Buys |
| Keepa | €29 | €290 | Product Finder, viewer quotas; free tier keeps charts and alerts |
| camelcamelcamel, Honey, Slickdeals, PriceSpy, PerfectRec | Free | Free | Nothing |
| Perplexity Pro | $20 | $200 | Shopping bundled |
| ChatGPT | Free tier has shopping | | |

Gating pattern: paid tiers gate test data and ratings, not price tracking or alerts, which are free everywhere.
Custom weighting is essentially ungated in the market. No conversion-rate disclosures found.

## Second category to test affiliate revenue
Amazon holds about 40 percent of US e-commerce and near 50 percent of online consumer electronics.
1. Robot vacuums (recommended): Amazon-dominant; AOV $300 to $1,000; huge review volume; 100 to 200 current
   models; Home category about 3 percent pre-cut; specs suit AI ranking; Walmart and brand-direct programs as a
   comparison arm.
2. Espresso machines and coffee makers: Kitchen 4.5 percent; AOV $100 to $800; brand-direct programs exist.
3. Headphones and earbuds: highest Amazon share but low AOV; tests traffic more than revenue.
Before launch confirm on Associates Central: current fee schedule, Creators API threshold, offsite cart rules.

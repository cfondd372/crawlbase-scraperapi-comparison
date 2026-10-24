# crawlbase crawling api: How It Works, What Requests Really Cost, and When ScraperAPI Is the Smarter Pick

The Crawlbase Crawling API is a managed scraping endpoint: you pass any URL plus your token, and it returns fully rendered HTML (or structured JSON) through a residential IP pool, with CAPTCHA and anti-bot handling done on their side. It's a legitimate product with real strengths — success-only billing, a genuinely free starting tier — but its advertised "$3.00 down to $0.02 per 1,000 requests" is the top of a rate card with difficulty multipliers stacked on top, and that's where most pricing rundowns stop short.

This guide walks through how the Crawling API actually works, what a request costs once the multipliers kick in, and where a competitor like ScraperAPI fits — especially if your targets are heavily protected sites.

> **Quick take:** If your targets are standard-complexity sites and you want pay-per-success flexibility, Crawlbase's pay-as-you-go model is hard to beat. If you're scraping Google, Amazon, LinkedIn, or sites behind Cloudflare/DataDome — or you want predictable flat monthly billing — ScraperAPI's plans from $49/month (100,000 credits, annual billing drops it to $44.10) are usually the better fit. 👉 [See ScraperAPI's full plan list and start a 7-day free trial](https://www.scraperapi.com/pricing?fp_ref=coupons)

## How the Crawlbase Crawling API works

You call one REST endpoint with two required parameters — `token` and `url` — and the service handles everything in between:

bash
curl 'https://api.crawlbase.com/?token=YOUR_TOKEN&url=https%3A%2F%2Fhttpbin.org%2Fheaders'


Each request routes through rotating residential IPs across 30 geographies (with `country=` targeting for 50+ countries), executes the page in a real browser when needed, and clears bot checks automatically. There's no proxy management and nothing to patch when a target site changes its layout.

Beyond raw HTML, the optional parameters are where the product gets useful:

- `autoparse=true` or `scraper=<name>` — returns structured JSON (title, content, prices, images, links). There are 100+ built-in site scrapers, plus `scraper=generic-extractor` for universal extraction on less common sites.
- `format=md` — clean Markdown output, handy if you're feeding an AI or RAG pipeline.
- `screenshot=true` — a full-page capture (costs 2 credits).
- `device=mobile` and `store=true` — mobile user agents, and cloud storage for every page you crawl.
- `tor_network=true` — crawls onion sites over Tor, which is unusual at this price point.

There's also an asynchronous Crawler with webhook delivery, and official SDKs for Python, Node, PHP, Go, and Ruby. New accounts start with 1,000 free credits, no card on file, and completing setup steps grows that to up to 5,000. The same token works across the Crawling API and every scraper.

## Crawlbase pricing: two ways to pay, one rate card

Crawlbase offers pay-as-you-go and subscriptions, and both draw from a single credit balance shared by every product — the Crawling API with screenshots, the Crawler, Smart AI Proxy, and cloud storage all bill against the same ladder.

**Pay as you go** has no monthly fee and no card required. You're billed in arrears for successful requests only, at a graduated rate:

| Volume band (per 1,000 successful requests) | Rate |
| --- | --- |
| First 1K | $3.00 |
| Next 10K | $2.00 |
| Next 100K | $0.60 |
| Next 1M | $0.50 |
| Next 10M | $0.10 |
| Beyond | down to $0.02 |

The company's own worked examples: 100K standard requests costs $76.40 (about $0.76 per 1,000), 1M costs $527.50, and 10M costs $1,471.90. You can set a hard spend cap, and usage pauses automatically when you hit it.

**Subscriptions** buy a monthly credit block at a lower effective rate, with overage continuing on the same ladder:

| Plan | Credits/mo | Monthly | Annual (1 month free, 8% off) |
| --- | --- | --- | --- |
| Starter | 200K | $99 | $1,089/yr (~$0.50/1k) |
| Scale 500K | 500K | $199 | $2,189/yr (~$0.40/1k) |
| Scale 1M (most popular) | 1M | $349 | $3,839/yr (~$0.35/1k) |
| Growth | 2M | $599 | $6,589/yr (~$0.30/1k) |

Failure billing is the genuinely fair part: a request that fails or gets blocked is never billed, regardless of the site or rendering mode. No long-term contracts, cancel any time.

## The credit math most comparisons skip

Here's the part that changes real-world budgets, and it's missing from most rundowns of this API.

**Site difficulty multipliers apply to your entire ladder total, not per request.** Every target site gets a tier — Standard ×1, Moderate ×1.5, Complex ×2, Complex I ×3, Complex II ×4.5, Extreme ×9, Extreme I ×13.5, Extreme II ×20. Crawlbase says most of the web is standard, which is fair, but heavily defended domains carry the big multipliers, and they're applied automatically.

**JavaScript rendering doubles everything.** Any request rendered in a real browser costs 2× its tier rate, whatever the site. There's no separate browser tier — it's a flat multiplier on top.

Stack those two: a JS-rendered request to an "Extreme" tier site burns 18 standard credits. If half your crawl volume lands on complex or extreme-tier domains with rendering enabled, your effective per-request cost can be several multiples of the headline rate. The failure-billing policy protects you from paying for blocked attempts, but successful requests against expensive tiers still add up fast.

The practical move: run your actual target URLs through the cost estimator on their pricing page before committing to a volume estimate, and set a spend cap so a surprise multiplier can't blow the monthly budget.

## If your targets are protected sites: ScraperAPI as the alternative

Crawlbase's own user feedback pattern is telling. Reviews on G2 and Capterra (4.5/5 across 900+ reviews) praise the free tier and fast setup, but a recurring complaint is difficulty with Cloudflare Enterprise and DataDome targets, with a large performance gap between simple and protected sites. A third-party comparison sums it up bluntly: "Solid budget scraping platform for standard sites. Not competitive for hardest anti-bot targets."

ScraperAPI is built for the opposite profile — an API-first layer where one call handles proxy rotation, CAPTCHA bypass, JS rendering, and IP rotation, with dedicated structured endpoints for Google, Amazon, and Walmart that skip parsing entirely. Its pricing is flat monthly credit buckets, which makes costs predictable in a way a multiplier ladder doesn't:

| Plan | Monthly | Annual (per mo) | API credits | Concurrent requests | Get started |
| --- | --- | --- | --- | --- | --- |
| Free | $0 | — | 1,000 | 5 | [Start free](https://dashboard.scraperapi.com/signup?fp_ref=coupons) |
| Hobby | $49 | $44.10 | 100,000 | 20 | [Start Trial](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Startup | $149 | $134.10 | 1,000,000 | 50 | [Start Trial](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Business | $299 | $269.10 | 3,000,000 | 100 | [Start Trial](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Scaling (most popular) | $475 | $427.50 | 5,000,000 | 200 | [Start Trial](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Professional | $975 | $877.50 | 10,500,000 | 300 | [Start Trial](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Advanced | $1,975 | $1,777.50 | 21,500,000 | 500 | [Start Trial](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Enterprise | custom | — | 22,000,000+ | 500+ | [Talk to sales](https://www.scraperapi.com/contact-sales/?fp_ref=coupons) |

Two billing details worth knowing before you pick a tier, because they're easy to miss on the pricing page:

- **Credits aren't 1:1 with requests.** A standard page costs 1 credit, Amazon costs 5, Google and Bing (including subdomains) cost 25, and LinkedIn costs 30. Sites behind Cloudflare, Datadome, or PerimeterX add 10 credits per request when the bypass succeeds. The dashboard includes a Domain Cost Estimator for any URL, and you can set a `max_cost` per scrape so a single request can't blow past your limit — the same discipline the Crawlbase spend cap gives you.
- **Overage is handled three different ways.** Exceeding your concurrent thread limit returns a 429 error with no charge (requests are rejected, not queued). When monthly credits run out, scraping pauses by default; Hobby through Professional can opt into auto-upgrade to the next tier, while Scaling and above can enable pay-as-you-go at a fixed per-credit rate with a monthly cap you set.

Every plan, including the trial and free tier, includes JS rendering, premium residential and mobile IPs, JSON auto-parsing, custom headers and sessions, automatic retries, unlimited bandwidth, and a 99.9% uptime guarantee. Geotargeting on Hobby and Startup is US & EU only — country-level targeting starts at Business — and analytics history is limited to 30 days below Business. The trial runs 7 days with 5,000 credits and no credit card, so testing your real targets costs nothing: 👉 [Try ScraperAPI free for 7 days with 5,000 credits](https://www.scraperapi.com/signup?fp_ref=coupons)

Third-party signals back the positioning: a 4.6/5 user rating across 850+ reviews, a 40M+ IP pool covering 50+ countries, and a #3 ranking in Scrapeway's May 2026 benchmark.

## Head-to-head: where each one wins

|  | Crawlbase Crawling API | ScraperAPI |
| --- | --- | --- |
| Billing model | Success-only, volume ladder + difficulty multipliers | Flat monthly credit buckets, annual saves 10% |
| Entry cost | $0 (pay-as-you-go, ~$0.76/1k at 100K) | $49/mo (or free tier: 1,000 credits) |
| IP pool | 140M residential (company figure) | 40M+ (third-party figure) |
| Known strength | Tor support, 100+ built-in scrapers, async Crawler | Structured endpoints for Google/Amazon/Walmart, bypass stack |
| Known weakness | Heavy multipliers on defended sites | 50+ country coverage, credit costs at high volume |
| Trial | 1,000–5,000 free credits, no card | 7 days / 5,000 credits, no card |

Note the honest caveats in both directions. Crawlbase's pay-per-success model is objectively less wasteful for messy crawl lists where many URLs fail — you pay nothing for the misses. ScraperAPI's credit multipliers mean Google scraping at 25 credits each runs through a 100K bucket in 4,000 successful requests, so budget accordingly. Neither claim cancels the other; they optimize for different failure modes.

## FAQ

**Does the Crawlbase Crawling API charge for failed requests?** No. The pricing page states plainly that a request which fails or is blocked is never billed, whatever the site or rendering mode. Billing applies to successful requests only.

**How much do 100,000 requests cost on Crawlbase?** For standard-complexity sites, $76.40 total (about $0.76 per 1,000) on the graduated ladder. That figure excludes difficulty multipliers and JavaScript rendering, both of which scale the total upward — JS rendering alone doubles it.

**Is Crawlbase or ScraperAPI better for protected sites?** ScraperAPI has the stronger case for hard targets: dedicated bypass logic for Cloudflare, Datadome, and PerimeterX, structured Google/Amazon/Walmart endpoints, and a top-3 Scrapeway benchmark ranking. Crawlbase reviews consistently flag Cloudflare Enterprise and DataDome as pain points. For standard sites at volume, Crawlbase's success-only ladder is usually cheaper.

**Can I try both for free?** Yes. Crawlbase gives 1,000 free credits (up to 5,000 after setup steps) with no card. ScraperAPI gives 1,000 credits on its free plan and a 7-day trial with 5,000 credits, also without a card.

## Which one should you actually pick

A few decision rules based on the verified numbers:

- **Prototype or low volume, standard sites:** Crawlbase pay-as-you-go at $0/month is the cheapest way to start, and failure billing means you waste almost nothing.
- **Scraping search engines, marketplaces, or LinkedIn:** ScraperAPI. Its built-in logic for those exact domains, plus the Domain Cost Estimator for budgeting, exists precisely because raw crawling on them is expensive and fragile.
- **Targets behind Cloudflare or DataDome:** ScraperAPI's bypass stack and benchmark track record make it the safer bet; pair its `max_cost` parameter with the free trial to verify success rates on your specific targets before paying.
- **Predictable monthly budget matters more than per-request cost:** ScraperAPI's flat tiers with 10% annual billing are easier to forecast than a multiplier-adjusted ladder.

One last check before you commit to either: pull five of your real target URLs, estimate their per-request cost under both models, and run the free credits against them. That test settles the pricing debate faster than any comparison table — and when ScraperAPI comes out ahead for your use case, 👉 [lock in annual billing on the Scaling plan at $427.50/month](https://www.scraperapi.com/signup?fp_ref=coupons) to get the 100K–5M credit tiers at their lowest published rate.

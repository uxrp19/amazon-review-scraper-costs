# Product Review Scraper Guide: Can You Still Scrape Amazon Reviews, What Does a Review Cost, and Where Does ScraperAPI Fit? (Plans, Credits and Limits)

Amazon review pages now require a logged-in session to show full review data, datacenter IPs get blocked the moment they touch a review page, and the tools that still return parsed rows charge about $3.90 per 1,000 reviews. ScraperAPI is not one of those tools, and its own documentation says so: the Amazon Reviews endpoint is retired because ScraperAPI doesn't scrape behind logins.

What it sells sits one layer lower. Rotating proxy pools, JS rendering, premium proxies, geotargeting, and auto-parsing on supported endpoints, all billed in credits where an Amazon request costs 5. That makes it a strange fit for someone searching for a review scraper, and a very good fit for a narrower group: people who already parse pages and only need Amazon to stop blocking them.

The rest of this covers what a credit actually buys on Amazon, how the credit curve compares with per-review pricing at 10,000 and 100,000 reviews, and which tier is overkill for your volume.

> **Quick verdict.** Want finished review rows for a handful of ASINs? Buy a per-review scraper at roughly $3.90 per 1,000 and skip ScraperAPI entirely. Already have parsing code and just need Amazon pages to arrive unblocked? The 7-day trial gives you 5,000 credits, which is 1,000 Amazon requests and no credit card, enough to measure your success rate before you pay anything. 👉 [Start the 7-day ScraperAPI trial with 5,000 credits](https://www.scraperapi.com/pricing?fp_ref=coupons)

## The plans and what one credit buys

ScraperAPI runs seven paid tiers plus a free trial. Credits are the unit of consumption, concurrent threads cap how many requests run at once, and geotargeting moves from US and EU only on the two cheapest tiers to country-level from Business upward.

| Plan | Monthly credits | Concurrent threads | Geotargeting | Price | Get it |
| --- | --- | --- | --- | --- | --- |
| Free trial | 5,000 (7 days) | — | US & EU | $0, no card | [Start the trial](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Hobby | 100,000 | 20 | US & EU | See pricing page | [See Hobby's current price](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Startup | 1,000,000 | 50 | US & EU | See pricing page | [See Startup's current price](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Business | 3,000,000 | 100 | Country-level | See pricing page | [See Business's current price](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Scaling | 5,000,000 | 200 | Country-level | $475/month, or $427.50/month billed annually ($5,130/year, saves $570) | [See Scaling's current price](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Professional | 10,500,000 | 300 | Country-level | See pricing page | [See Professional's current price](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Advanced | 21,500,000 | 500 | Country-level | See pricing page | [See Advanced's current price](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Enterprise | 22,000,000+ | 500+ | Country-level | See pricing page | [See Enterprise's current price](https://www.scraperapi.com/pricing?fp_ref=coupons) |

Scaling is the only tier whose list price was confirmed at the time of writing, so the price column is left blank for the others rather than filled in from memory. ScraperAPI quotes each plan separately and the per-credit rate improves as you move up, so the gap between two neighbouring tiers is smaller than the gap in credits suggests.

Credits get consumed at different rates depending on what you ask for:

| Request type | Credit cost |
| --- | --- |
| Standard page | 1 |
| Amazon, Walmart, eBay | 5 |
| Google or Bing, including subdomains | 25 |
| LinkedIn | 30 |
| Bot-protection bypass (Cloudflare, DataDome, PerimeterX/HUMAN, Akamai) | +10 |
| JS rendering (`render=true`) | +10 |
| Premium proxies (`premium=true`) | +10 |
| Premium proxies plus JS rendering | 25 total |
| Ultra premium (`ultra_premium=true`, paid plans only) | 30 |
| Ultra premium plus JS rendering | 75 |
| Geotargeting, sessions, device type, output format, auto parsing | 0 |

The number that matters for review work is **5**. Amazon is priced as an e-commerce domain at 5 credits per request, the same as Walmart and eBay, five times a standard page and a fifth of a Google SERP. Every response returns its cost in a `sa-credit-cost` header, and the Domain Cost Estimator in the dashboard prices a URL before you send it, so you can check whether a marketplace you haven't tried bills at 5 or more. 👉 [Check the current per-plan credit allowance](https://www.scraperapi.com/pricing?fp_ref=coupons)

## What actually blocks a product review scraper

Start with the vendor retreat. ScrapeHero, which sold Amazon scraping tools and tutorials, posted a notice on its Amazon reviews guide in March 2024: Amazon had updated its review access policies and now required authentication to view full review data, so the company retired the scrapers and APIs previously published in that article and began looking for policy-friendly alternatives.

That's one company's business decision, not a law of physics. Apify actors were still selling Amazon review extraction in 2026, and their own listings explain the cost of doing it. One actor states plainly that Amazon aggressively blocks datacenter IP addresses on review pages and that residential proxies are required; on the free Apify tier you have to supply your own proxy URL or the run fails outright. A second actor uses 8+ rotating user agents, random delays of 1 to 3 seconds between requests, exponential backoff retries at 2, 4 and 8 seconds capped at 3 attempts, fallback CSS selectors per field, and CAPTCHA detection that triggers a retry.

Both are describing the same wall. Amazon scores IP reputation, validates browser fingerprints, and serves CAPTCHA challenges. A scraper rotating datacenter IPs dies on the first page, which is why every working setup in 2026 pays for residential exits. ScrapeHero's own tutorial code shows what a block looks like from the client side: an HTTP status above 500 combined with the string "To discuss automated access to Amazon data please contact" somewhere in the response body.

So the honest answer to "can I still scrape Amazon reviews" is that publicly visible reviews are reachable, full history behind a login is not something most infrastructure vendors will touch, and anyone promising complete review history is doing session work you should test before you trust.

## Where ScraperAPI fits, and the one thing it won't do

ScraperAPI's documentation is explicit about the review endpoint: "Amazon review pages currently require a login to access. Since ScraperAPI does not support scraping behind a login, this endpoint is not available." The same page directs users to the Amazon Product Page API instead, for the reviews that are publicly available on a product page.

That settles the positioning question. ScraperAPI is a fetch and unblocking layer with structured parsing on supported endpoints. It is not a product review scraper in the sense the search term implies, and anyone buying it expecting a JSON array of 5,000 reviews for an ASIN will be disappointed.

What you get instead:

- Amazon product page HTML, at 5 credits per request, with rotating proxies and optional JS rendering
- Premium and ultra premium proxy pools when the standard pool gets blocked, at +10 and +30 credits respectively
- Geotargeting at country level on Business and above, which matters when the review you want is on amazon.de rather than amazon.com
- Auto-parsing on endpoints that support it, at no extra credit cost

What you build yourself is the parser: selectors for review text, star rating, date, verified badge and reviewer name, plus the logic that detects a login wall and stops instead of writing empty rows into your dataset. If your target is Amazon's public review pages, budget for that work. If your target is any other retail site with reviews rendered in HTML, the same five-credit request returns the page and your existing parser handles the rest.

## What a review costs: $3.90 per 1,000 against credits

Managed review scrapers publish a per-review price. The current benchmark is **$3.90 per 1,000 reviews**, billed only on successful extractions, with blocked requests, empty products and duplicate rows free. Scaled up, that's $39 for 10,000 reviews, $390 for 100,000 and $3,900 for a million. For sentiment analysis on a category, $390 for 100,000 rows with rating, text, date, verified flag, helpful votes and image URLs is a defensible line item, especially since the vendor absorbs the proxy bill and the retry logic.

Credits work on a different curve. At the Scaling rate of $475 for 5,000,000 credits, one credit costs $0.000095. An Amazon request at 5 credits costs **$0.000475**, or about $0.48 per 1,000 requests. Add premium proxies on every request and you're at 15 credits, or $0.001425 per request, roughly $1.43 per 1,000 requests.

Compare that to $3.90 per 1,000 reviews and it looks like a bargain. It isn't a like-for-like comparison, and the difference is the work. A review page returns a pageful of reviews, so 1,000 reviews is far fewer than 1,000 requests, but you pay for the parsing, the login-wall detection, and the maintenance when Amazon changes its markup. The per-review vendor has already paid those costs and prices them into the $3.90.

Where the crossover sits depends on your volume and your engineering time, not on the price list. Below roughly 10,000 reviews, a $39 managed bill is cheaper than any monthly plan plus a week of your attention. Past 100,000 reviews, the marginal cost of a credit-based approach is small enough that a working parser pays for itself. 👉 [Run your own numbers against ScraperAPI's plans](https://www.scraperapi.com/pricing?fp_ref=coupons)

## Which setup fits your volume

**You want 50 to 500 reviews to test a hypothesis.** Don't buy a plan. Either use a managed scraper's free tier or spend trial credits. ScraperAPI's 5,000 trial credits buy 1,000 Amazon requests in seven days, which is enough to find out whether your parser survives a login wall or a CAPTCHA page on the marketplaces you care about.

**You want 10,000 to 50,000 reviews for a sentiment model.** A per-review vendor costs $39 to $195 and hands you typed fields. On the ScraperAPI side, Hobby's 100,000 credits buy 20,000 Amazon requests, or 6,666 if you need premium proxies on each one. Neither path is wrong; the managed one is faster to first data.

**You need 500 ASINs, 12 marketplaces, and JSON with verified-purchase flags.** Buy the specialist. Amazon review actors cover .com, .co.uk, .de, .fr, .it, .es, .ca, .com.au, .co.jp, .in, .com.br and .com.mx, accept hundreds of ASINs or product URLs per run (up to 500 products), and filter by star rating including positive and critical buckets, date range, verified purchases only, and keyword inside the review text. Output fields include `asin`, `rating`, `title`, `body`, `date`, `isVerifiedPurchase`, `helpfulVotes`, `images`, `variant`, `countryReviewed` and `marketplace`. Rebuilding that field map yourself is a project.

**You already parse, and you're getting 503s.** This is ScraperAPI's case. Five credits per Amazon request against a proxy pool you don't maintain, with `max_cost` set per scrape so a single stubborn URL can't quietly drain the balance. Watch concurrency: Hobby's 20 threads is thin for a batch run, and the platform returns a 429 rather than queuing when you exceed it.

**You need product context alongside the reviews.** One Apify actor caps reviews at 500 per product with a default of 50, and outputs price, BSR and category, bullet points, images, variations and Q&A in the same record. ScraperAPI's Amazon Product Page endpoint at 5 credits covers the same ground if you're willing to parse it.

**Your target isn't Amazon.** Shopee review actors exist with 27 fields per review, per-aspect sub-ratings, seller replies and a per-product summary row, capped at roughly 14,000 reviews on high-volume listings. Their free tier is 20 runs lifetime at 20 reviews each, which tells you how the industry treats free samples.

## Building it yourself: what the free repos leave out

The open-source route is real and the code is readable. One maintained project runs on Python 3.12 with BeautifulSoup 4 and Poetry, accepts a list of ASINs and keywords, supports verified-only, most-recent and star filters plus English-only reviews, and writes CSV or JSON. It also requires a paid proxy subscription, with SmartProxy and Oxylabs Web Unblocker wired in and username and password required, and it carries an educational-purposes disclaimer.

ScrapeHero's tutorial version uses Python Requests, LXML, Dateutil and Selectorlib, with CSS and XPath selectors in a `selectors.yml` file, URLs from `urls.txt` and output to `data.csv`. The tutorial itself warns that dynamic content and anti-scraping measures make this painful for beginners.

Neither repo solves CAPTCHA. Both assume you bring a proxy. Neither survives an Amazon markup change without selector edits. That's the real cost of DIY, and it's why so many people end up paying either $3.90 per 1,000 reviews or a monthly credit bill.

## FAQ

**Can you still scrape Amazon reviews at all?**
Publicly visible reviews, yes, with residential proxies and retry handling. Full review history behind a login is the part vendors are backing away from. ScraperAPI won't touch it, ScrapeHero retired its tools over it, and the actors still selling it are selling their session and proxy handling rather than a guarantee. Test a sample on your target marketplace before committing to a volume.

**Why does everyone insist on residential proxies?**
Because Amazon blocks datacenter IP ranges on review pages immediately. Residential proxies route through real ISP addresses, which Amazon treats as ordinary browser traffic. It doesn't make you invisible; it moves you from "blocked on sight" to "challenged sometimes."

**Is per-review pricing or a monthly credit plan cheaper?**
Under about 10,000 reviews, per-review pricing wins because you're not paying for engineering. Above 100,000, credits win on marginal cost, provided you have a parser that works. The break-even point isn't a price on a page; it's how much your time costs.

**Can I pull only one-star reviews?**
Managed Amazon actors filter by star at the request level, including critical-only and positive-only buckets, plus date ranges and verified purchases. If you're on ScraperAPI's raw HTML path, filtering happens after parsing unless the endpoint you're calling exposes a filter parameter.

**Will the output drop straight into pandas?**
From a managed review actor, yes: one JSON object per review with separate rating, title, body, date and verified fields. From ScraperAPI you get HTML unless you call a structured endpoint, so the DataFrame comes after your parser runs.

## Pick by volume, then check two things before checkout

If you need 500 reviews to sanity-check a pricing decision, use a free tier and don't spend anything. If you need 50,000 rows with typed fields and no maintenance, pay the $3.90 per 1,000 and move on. If you have working parser code and Amazon is the only thing standing between you and the data, ScraperAPI's credit model is the cheaper long-run option, and the trial is sized to prove it: 1,000 Amazon requests in seven days.

Two things to confirm before you enter a card. First, whether unused credits roll over past the billing month, which isn't stated on the pricing page and matters if your scraping is bursty. Second, what happens at the limit: ScraperAPI returns a 429 and turns the request away without charging, then pauses until credits renew. Auto-upgrade to the next tier is opt-in on Hobby through Professional, and pay-as-you-go past your cap is available on Scaling and above, which is a meaningful difference if you'd rather not have a batch job stall mid-run.

👉 [Compare ScraperAPI plans and start the 7-day trial](https://www.scraperapi.com/pricing?fp_ref=coupons)

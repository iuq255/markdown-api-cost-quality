# Website to Markdown API: How to Judge Output Quality, What 1,000 Pages Really Costs in Credits, and Which ScraperAPI Plan Covers It

Every website to markdown API looks identical in a curl example: post a URL, get Markdown back. The differences sit in what the service had to do to the page before converting it, and how that work is billed. ScraperAPI returns Markdown through `output_format=markdown` on the same endpoint it uses for HTML scraping, and the format costs nothing extra on its own. Credits go to the fetch, and to JavaScript rendering or bot-protection bypass when a page demands them, at 10 credits each rather than 1.

That single number decides most of your bill. Below are the full plan lineup, the credit math for 1,000 pages once rendering is involved, and the parts of markdown extraction that no endpoint solves for you.

> Most teams extracting article or documentation pages should start on **Hobby**: 100,000 credits a month and 20 concurrent threads. A 7-day trial with 5,000 credits runs without a card, which is enough to test your real URLs and see the `sa-credit-cost` header on each response. 👉 [Check current credit allowances and prices for every plan](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## Four things that decide whether a markdown API is worth paying for

**Does it fetch, or only convert?** Turndown and Readability are libraries that turn HTML you already have into Markdown. A hosted endpoint also has to retrieve the page through proxies, retry the failures and execute JavaScript. That retrieval is what you're buying.

**Which path produced the output?** A static fetch, a headless browser and a site-specific adapter can return three different Markdown files from the same URL. If the API doesn't report which one ran, a nav-heavy or empty result is expensive to debug.

**Is rendering priced separately?** "1 credit per page" is the common headline. Behind it, headless rendering can cost one credit, three, or ten, and that changes the cost per 1,000 pages by an order of magnitude.

**What does a failed or empty page cost?** Some services charge nothing for failures; others bill the same as a success. On a batch of 50,000 URLs with a 10% failure rate, the difference is 5,000 credits.

## ScraperAPI's markdown output: one parameter, three limits

Add `output_format=markdown` next to `api_key` and `url`, or `output_format=text` if you want plain text. It works on the synchronous endpoint and on the async jobs endpoint. The documented proxy mode does not support these output formats yet, so a proxy-mode integration has to switch endpoints to get Markdown.

The conversion is free. ScraperAPI's credit table lists geotargeting, sessions, device type, output format and auto parsing at no additional cost, so Markdown from a standard page costs the same 1 credit as its HTML. What isn't free is everything a difficult page requires:

| Request type | Credits |
| --- | --- |
| Standard page | 1 |
| E-commerce (Amazon, Walmart, eBay) | 5 |
| SERP (Google, Bing, including subdomains) | 25 |
| Social (LinkedIn) | 30 |
| Bot-protection bypass (Cloudflare, DataDome, PerimeterX/HUMAN, Akamai, Turnstile) | 10 |
| JS rendering (`render=true`) | 10 |
| Premium proxies (`premium=true`) | 10 |
| Screenshot | 10 |
| Premium proxies + JS rendering | 25 |
| Ultra premium | 30 |
| Ultra premium + JS rendering | 75 |

Two operational details matter if you plan to run this at volume. Every response carries a `sa-credit-cost` header showing what that request actually consumed, and `max_cost` caps a single scrape so an unexpectedly expensive domain can't drain an allowance overnight. The dashboard's Domain Cost Estimator prices a URL before you send traffic at it.

## All ScraperAPI plans

ScraperAPI prices by credit volume; the dollar amount attached to each tier depends on billing cycle and any running promotion, so pull the current figure from the pricing page rather than from a table that may be a month stale. Annual billing takes 10% off on every plan except Enterprise, which is quoted individually.

| Plan | Credits/month | Concurrent threads | Geotargeting | Billing notes | Start |
| --- | --- | --- | --- | --- | --- |
| Free | 1,000 | 5 | Not listed | No card required | [Free plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Hobby | 100,000 | 20 | US & EU | Auto-upgrade opt-in, no rollover | [Hobby pricing](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Startup | 1,000,000 | 50 | US & EU | Auto-upgrade opt-in, no rollover | [Startup pricing](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Business | 3,000,000 | 100 | Country-level | Auto-upgrade opt-in | [Business pricing](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Scaling | 5,000,000 | 200 | Country-level | Pay-as-you-go available | [Scaling pricing](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Professional | 10,500,000 | 300 | Country-level | Pay-as-you-go available | [Professional pricing](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Advanced | 21,500,000 | 500 | Country-level | No auto-upgrade | [Advanced pricing](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Enterprise | 22,000,000+ | 500+ | Country-level | Custom terms | [Enterprise contact](https://www.scraperapi.com/pricing/?fp_ref=coupons) |

Three rules in that table affect planning more than the credit numbers do. Credits never roll over, so an allowance sized for a peak month is wasted in a quiet one. Auto-upgrade exists on Hobby through Professional but not on Advanced or Enterprise. Pay-as-you-go, which lets you keep scraping past your limit at a fixed per-credit rate up to a cap you set, only appears from Scaling upward. On Hobby, Startup and Business, an exhausted allowance simply pauses until renewal unless auto-upgrade is enabled.

Analytics history runs 30 days on Hobby and Startup and is unlimited from Business up. If you're debugging why one section of a site returns thin Markdown, that retention window is the practical difference between the two groups.

## What 1,000 pages actually costs

The headline credit count and the workable page count diverge fast once rendering enters the picture.

| Page type | Credits per page | Credits per 1,000 pages | Pages per 1,000,000 credits |
| --- | --- | --- | --- |
| Static HTML | 1 | 1,000 | 1,000,000 |
| JS-rendered SPA | 10 | 10,000 | 100,000 |
| Behind Cloudflare or DataDome | 10 | 10,000 | 100,000 |
| E-commerce listing | 5 | 5,000 | 200,000 |
| SERP | 25 | 25,000 | 40,000 |
| Ultra premium + rendering | 75 | 75,000 | 13,333 |

Hobby's 100,000 credits is 100,000 static pages, 10,000 rendered pages, or roughly 1,333 pages at the ultra-premium-with-rendering rate. A content migration off a docs site built in static HTML and a competitive-intelligence pipeline over React dashboards are not the same purchase, even though both are "website to markdown."

The same arithmetic applies to the 5,000-credit trial: it's 5,000 test pages or 500, if every URL needs a browser. Sample the messy half of your URL list first, not the easy half. 👉 [See which plan's allowance matches your rendering mix](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## How the markdown options compare on billing

Rendering is where these services diverge, and it's rarely the number in the headline.

| Service | Markdown entry point | Published price | Rendering in that price? |
| --- | --- | --- | --- |
| ScraperAPI | `output_format=markdown` on the scraping endpoint | Per-plan credit volume, 10% annual discount | No, `render=true` adds 10 credits |
| Cloudflare Browser Rendering | `POST /markdown`, accepts `url` or `html` | Not published on the endpoint docs | Rendering is the product; no separate line item shown |
| Upscrape | `POST /markdown` | 1 credit per call, failed calls free | Browser fallback on by default, no surcharge listed |
| Geekflare | `format: ["markdown"]` or `["markdown-llm"]` on the scraping endpoint | Free 500 credits/mo, Starter $19/mo for 10K, Growth $69/mo for 100K, Business $349/mo for 1M | Headless Chrome included at 1 credit per page |
| mdget.dev | `GET /<url>` | Free 1,000 credits/mo, Pro 50,000 credits/mo | A live browser render costs 3 credits |
| UrlReader | Self-hosted `GET /api/scrape` | Open source, no hosted price | You supply the Playwright instance |

If your URL list is mostly static article HTML, ScraperAPI's economics match the cheapest options in that table. If most of your URLs are single-page apps, budget 10 credits per page and compare against services that fold rendering into one credit. Geekflare's page also claims its `markdown-llm` variant produces roughly 60% fewer tokens by stripping navigation, ads and cookie banners; that figure is the vendor's own and hasn't been independently verified, but the underlying idea is worth testing on your own corpus, since boilerplate removal changes chunk quality more than it changes cost.

Document inputs are a separate question. Upscrape documents PDF, Word, Excel, CSV, JSON, XML and RSS/Atom conversion explicitly. ScraperAPI's markdown parameter is documented for page requests, so if your pipeline is mostly PDFs, check that path before committing to a credit tier.

## Output quality is a rendering problem, not a conversion problem

Cloudflare's own documentation is the clearest illustration: the default page-load behavior can return empty or incomplete results on JavaScript-heavy pages, because the browser considers the page loaded before the framework has painted the content. The fix is `gotoOptions.waitUntil` set to `networkidle0` or `networkidle2`, or `waitForSelector` on a specific element if you know which one signals that the content has arrived. Nothing about the Markdown conversion was broken; the input was.

That failure mode is invisible in a smoke test on example.com and obvious on a real SPA. Two habits catch it early:

Log which path produced each result. mdget.dev exposes an `X-Markdown-Method` header with values like `native`, `readability+turndown`, `browser+readability+turndown`, `jina` and `firecrawl`, so a sudden drop in quality can be traced to a fallback path rather than guessed at. ScraperAPI gives you the equivalent signal on cost through `sa-credit-cost`: a page that quietly stopped rendering shows up as 1 credit instead of 10.

Check the body, not just the status code. A 200 response containing a title, a cookie banner and nothing else is the most common markdown extraction failure, and it poisons a RAG index silently. Compare heading counts and body length against a manual sample of 20 URLs before you run the full batch.

## Who shouldn't buy a credit-based markdown API

**One-off conversions.** A few hundred pages, once, doesn't justify a subscription. mdget.dev's anonymous tier handles 10 requests a minute and 100 a day per IP with cached fetch and Readability, no browser. Geekflare's free tier gives 500 credits a month.

**Teams already on Cloudflare Workers.** The Browser Rendering binding calls the markdown endpoint without an API token, which removes a key-management step entirely.

**PDF-heavy pipelines.** Look at a service that documents document conversion rather than a page scraper.

**Privacy-sensitive internal pages.** UrlReader runs on your own infrastructure with Playwright and Turndown, with parameters for stripping tables, images and links, and a configurable wait before parsing. It returns a 500 for every error type, so error handling is coarse, but nothing leaves your network.

## Questions that come up before signing up

**Do I need a separate markdown endpoint or API key?** No. It's a parameter on the existing request: `output_format=markdown`. The same key, the same endpoint, the same credit pool.

**Does markdown output cost more than HTML?** No. Output format is listed as a free option. What costs more is `render=true` at 10 credits, and bot-protection bypass at 10 credits.

**How do I handle a React or Vue site that returns an empty body?** Set `render=true` and accept the 10-credit cost, then verify on a sample that the body is actually populated. If a page still comes back thin, check whether it sits behind bot protection, which adds another 10 credits to bypass.

**What happens when I run out of credits?** Scraping pauses until renewal by default. Auto-upgrade, available on Hobby through Professional, moves you to the next tier when the allowance empties. Pay-as-you-go, from Scaling up, keeps requests running at a fixed per-credit rate under a cap you set. Going over your concurrency limit returns a 429 rather than an extra charge, so a burst of parallel workers fails fast instead of running up a bill.

**Is there a refund or trial?** The trial is 7 days and 5,000 credits with no card, and the free plan continues at 1,000 credits a month with 5 threads afterward. 👉 [Start on the free plan or the 7-day trial](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## Picking a tier

Work out your credits per page first, then pick the smallest plan whose allowance covers a month of that number.

Mostly static articles and docs, under 100,000 pages a month: Hobby. Same content at ten times the volume: Startup. Mostly SPAs or protected sites at 10 credits per page: divide your monthly page count by 10 before comparing tiers, because a 1,000,000-credit plan is a 100,000-page plan under those conditions. Need country-level geotargeting rather than US and EU only: that starts at Business. Expecting traffic spikes you don't want to pre-pay for: Scaling or above, where pay-as-you-go exists.

Before you check out, confirm two things on the pricing page: the dollar figure for the tier you picked on your billing cycle, and whether auto-upgrade is on, since that setting decides whether a busy month silently moves you up a tier or stops your jobs. 👉 [Compare ScraperAPI's plans before you commit to a credit tier](https://www.scraperapi.com/pricing/?fp_ref=coupons)

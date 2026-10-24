# residential proxy services: what per-GB pricing actually buys you, and when a managed scraping API like ScraperAPI costs less

Most "best residential proxy services" lists rank the same per-GB providers — IPRoyal, Bright Data, Decodo, Webshare — and stop there. That's useful if you already run your own crawler and just need IP traffic. But if what you actually want is data off protected pages, there's a second category these lists usually bury: managed scraping APIs, where you pay per request instead of per gigabyte and the provider handles rotation, retries, CAPTCHAs and JavaScript rendering for you.

The price gap between the two models is bigger than most people expect. At modest volume, residential bandwidth runs roughly **$2.25–$3.75/GB**. A managed API like ScraperAPI charges per successful request — about **$0.49 per 1,000 pages** on the entry Hobby plan, dropping to roughly $0.095/1k at the 5-million-credit tier. For typical ~1MB pages, that works out to roughly 10–30x less than raw residential bandwidth, and the API does more of the work.

This article walks through both models with real numbers, then digs into ScraperAPI's full plan list, the credit multipliers that decide your true per-page cost, and the limits (session length, geo targeting, no IP-level control) that should decide whether you buy it or a traditional proxy pool.

> **Quick take:** if your goal is scraping standard e-commerce and search pages and you don't want to build proxy rotation yourself, ScraperAPI's **Hobby plan at $49/month (100,000 credits, 20 concurrent threads)** is the sensible starting point, with a 7-day / 5,000-credit free trial to test first. Raw per-GB proxies only make sense if you need long-lived IPs or full control of your own pipeline. 👉 [Check ScraperAPI's current plans and pricing](https://www.scraperapi.com/pricing?fp_ref=coupons)

## Two different products hide behind "residential proxy services"

A residential proxy service sells you traffic: you rent access to IPs that ride on real end-user connections, priced per GB, and your own code does the fetching, retrying and CAPTCHA-solving. A managed scraping API sells you finished pages: you send a URL, it returns HTML or parsed JSON.

This distinction matters because the billing axes are incomparable on sight. One recent pricing breakdown put it plainly: managed APIs charge per successful response, raw proxies charge per GB — and for ~1MB HTML pages, that makes the managed route often 10–30x cheaper than raw residential bandwidth, while also handling the plumbing.

When do raw proxies still win? Two cases: when you need full control of your own Crawlee/Scrapy pipeline, or when your workflow needs long-lived sessions on the same IP. If your targets aren't behind aggressive bot protection at all, the cheapest option is $0 — no proxy needed, since datacenter IPs run around $0.018–$0.03 per IP per month, roughly 100–300x cheaper than residential bandwidth.

Here's how the main providers compare on verified list prices:

| Provider | Billing model | Entry pricing | Notable policy |
| --- | --- | --- | --- |
| IPRoyal | Per GB | $1.75/GB at high volume | Pay-as-you-go, unused traffic never expires |
| Webshare | Per GB | $2.25/GB at 100 GB | Also sells datacenter pools (100 IPs for $2.99/mo) |
| Decodo (ex-Smartproxy) | Per GB | $3.75/GB entry | Residential pool with geo targeting |
| Bright Data | Per GB | ~$8/GB list price | Premium tier; Web Unlocker at $1.50/1k |
| Zyte | Per 1k requests | From $0.13/1k | Managed API, simple-site tiers |
| ScraperAPI | Per 1k credits | $0.49/1k (Hobby) | Managed API with rendering and unblocking included |

Note the structural difference in IPRoyal's favor: its traffic credits never expire, while most residential plans reset monthly and forfeit unused GB. If your scraping is seasonal, that detail matters more than the headline rate.

## ScraperAPI plans and pricing: the full list

ScraperAPI currently publishes seven paid plans plus a free tier. Every plan includes the same core feature set — JS rendering, premium residential and mobile IPs, CAPTCHA and anti-bot bypass, JSON auto-parsing, DataPipeline access, unlimited bandwidth, automatic retries and a 99.9% uptime guarantee — so what you're buying at higher tiers is volume, concurrency and geo granularity.

| Plan | Monthly price | Annual (10% off) | Credits | Concurrent threads | Geo targeting | Get it |
| --- | --- | --- | --- | --- | --- | --- |
| Free | $0 | — | 1,000 credits, refreshes monthly | 5 | US & EU | [Sign up free](https://www.scraperapi.com/?fp_ref=coupons) |
| Hobby | $49 | $44.10/mo | 100,000 | 20 | US & EU | [Get Hobby](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Startup | $149 | $134.10/mo | 1,000,000 | 50 | US & EU | [Get Startup](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Business | $299 | $269.10/mo | 3,000,000 | 100 | Country-level | [Get Business](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Scaling | $475 | $427.50/mo | 5,000,000 | 200 | Country-level | [Get Scaling](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Professional | $975 | $877.50/mo | 10,500,000 | 300 | Country-level | [Get Professional](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Advanced | $1,975 | $1,777.50/mo | 21,500,000 | 500 | Country-level | [Get Advanced](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Enterprise | Custom | Custom | 22,000,000+ | 500+ | Custom | [Contact sales](https://www.scraperapi.com/pricing?fp_ref=coupons) |

A few billing rules that don't show up in the plan grid:

- **Credits don't roll over.** Unused credits expire at renewal. Annual plans can renew early for a fresh batch if you burn through early.
- **Overage handling depends on tier.** By default, scraping pauses when credits run out. Hobby through Professional can opt into auto-upgrade, which bumps you to the next plan automatically. Scaling and above get true pay-as-you-go at a fixed per-credit rate, capped at a monthly spending limit you set.
- **Concurrency is hard-enforced.** Exceeding your thread limit returns a 429 and the request is rejected, not queued — no extra charge, but no free overflow either.

If you're unsure which tier fits, the trial exists for exactly this reason: 👉 [Start the 5,000-credit, 7-day free trial](https://www.scraperapi.com/?fp_ref=coupons) — no credit card required — and run your real target URLs before committing. The free plan's 1,000 credits refresh monthly and are enough to verify the integration works, not to run anything real.

## The real cost per page: credit multipliers decide everything

The most important thing to understand before buying rarely makes it into a review's opening paragraphs: ScraperAPI charges in API credits, and a "page" is not a fixed price. The official rule: a standard page costs 1 credit, **Amazon costs 5, Google and Bing cost 25, LinkedIn costs 30**, and sites behind Cloudflare, DataDome or PerimeterX add 10 credits per request when the bypass fires. On top of that, `render=true` (headless browser) costs about 10 credits, and premium/ultra-premium proxy tiers run 10–30 credits per request.

Do the math on a realistic job — say, Amazon search results pages with JS rendering enabled — and one million credits covers roughly 40,000–100,000 actual pages, not a million. That's the gap between the plan's headline number and what you get, and it's the most common complaint among operators who use the service: the credit system prices hardest exactly the jobs people sign up for.

Three mitigations are built in, and you should use all of them before upgrading:

1. The **Domain Cost Estimator** in the dashboard shows the exact per-URL cost before you scale up.
2. The `max_cost` parameter caps the credits any single scrape can consume.
3. Structured Data endpoints (Amazon, Google, Walmart and more) return parsed JSON and can be cheaper than rendering full pages yourself.

Plain-HTML workloads are where the pricing looks genuinely good: from Startup upward, standard pages work out to about $0.10–0.15 per 1,000 pages. Price monitoring across thousands of product pages with no JS rendering fits this profile, and the concurrency tiers (20/50/100 threads) handle that throughput without ever touching the premium multiplier. 👉 [Run the Domain Cost Estimator on your target URLs after signing up](https://www.scraperapi.com/?fp_ref=coupons) — the multiplier, not the sticker price, is what determines whether a plan is cheap.

## Where ScraperAPI fits — and where it doesn't

The honest fit profile, based on how the product is built: solo developers and small teams running price monitoring, SERP rank tracking or lead-gen scraping against standard e-commerce and search targets, at roughly 20,000–200,000 pages per month, who don't want to maintain their own proxy rotation and retry logic. Registration gets you an API key immediately, and a single GET request works within minutes:


curl "https://api.scraperapi.com?api_key=YOUR_KEY&render=true&country_code=us&url=https://example.com/page"


Now the limits, because they're dealbreakers for some use cases:

- **Sticky sessions are short.** The `session_number` parameter keeps the same IP within a session, but sessions are time-limited — published figures put the cap somewhere between 15 minutes and an hour. Whichever value applies to your plan, the takeaway is the same: this rules out ad verification and long-running account workflows that need the same identity for hours or days. For those, a dedicated residential or mobile provider is the better tool.
- **Geo targeting is paywalled at country level.** Hobby and Startup are locked to US and EU. Targeting any other country requires Business at $299/month or higher. This is a real constraint if your targets are, say, Japanese or Australian e-commerce sites on a small budget.
- **No IP-level control, by design.** You choose country, session and IP tier, but never a specific IP, ASN or city. If you need that granularity, you want raw proxies instead.
- **Response times vary.** Simple pages return in seconds; heavily protected domains can take up to 60 seconds (the docs recommend a 70-second client timeout). Use the Async API for batch volume.

The 2026 product has also grown well past the "proxy API with extras" label. There are structured data endpoints returning parsed JSON for Amazon, Walmart, eBay, Home Depot, Google properties, Redfin and even AI answer engines; an Async API with callbacks; DataPipeline, a low-code runner that takes up to 100,000 URLs or ASINs per project with scheduling and webhook delivery; a link-following crawler; and an MCP server that plugs into Claude Code, plus LangChain, LlamaIndex, n8n and Zapier integrations. ScraperAPI also acquired Traject Data this year and folded its Rainforest and SerpWow SERP/e-commerce APIs onto the same credit system.

## FAQ

**Is ScraperAPI a residential proxy service?**
It uses residential proxies under the hood — its pool advertises 40M+ IPs across residential, datacenter and mobile networks in 50+ countries — but packages them as a per-request API rather than selling raw bandwidth.

**Do ScraperAPI credits expire?**
Yes. Credits refresh with each billing cycle and don't roll over. If you routinely leave credits unused, a smaller plan plus pay-as-you-go overflow (available from Scaling up) beats paying for a tier you don't fill.

**What happens when I hit my concurrency limit?**
Requests get a 429 response and are rejected. Nothing queues and nothing gets overcharged; you either pace your requests, enable auto-upgrade, or move up a tier.

**Can I try it before paying?**
Yes — 5,000 credits over 7 days with no credit card, plus a standing free plan of 1,000 credits per month at up to 5 concurrent connections.

**Per-GB or per-request — which is cheaper?**
For pages around 1MB, per-request managed APIs typically land 10–30x cheaper than residential bandwidth for the same job. Raw per-GB proxies win when pages are tiny, when you need session persistence beyond an hour, or when you need to pick specific IPs or ASNs.

## The deciding factors, in order

Work through this list and the choice usually makes itself:

- **Standard targets, want it handled end-to-end** → ScraperAPI, starting with the trial, then Hobby or Startup. If your annual volume is predictable, the 10% yearly discount shaves the Hobby plan to $44.10/month.
- **Google, LinkedIn, or heavily-protected sites at real volume** → the 25x–30x credit multipliers will likely make this pricier than a per-GB residential plan, so compare the math carefully before defaulting to a managed API.
- **Need the same IP for hours, or specific ASN/IP control** → a raw residential provider with long sticky sessions fits better; IPRoyal's non-expiring PAYG traffic is the standout policy there, and Webshare's $2.25/GB at 100 GB undercuts most of the market.
- **Sites that aren't actually blocked** → don't buy either; datacenter pools or plain requests cost a fraction.

One last check before you buy anything: list your three most-scraped URLs, find their credit costs in the dashboard's Domain Cost Estimator, and multiply by your monthly volume. If the numbers work, 👉 [sign up for ScraperAPI and run the free trial on your real targets](https://www.scraperapi.com/?fp_ref=coupons) — it's the cheapest way to find out whether the managed model fits your workload, and it costs you nothing if it doesn't.

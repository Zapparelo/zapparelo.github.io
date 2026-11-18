---
title: "How to monitor competitor websites for new pages using their sitemap"
description: "We read the sitemaps of 30 SaaS websites on 18 Nov 2026: 29 had one, 6 had no lastmod dates at all, and on several the dates were useless. Here is a new-page monitor that works anyway."
date: 2026-11-18
lang: en
tags: [competitor monitoring, sitemap, seo, new pages, website monitoring]
layout: post
permalink: /en/monitor-competitor-new-pages-sitemap/
---

When a competitor launches a feature, a pricing plan, an integration or a landing page for your keyword, the new URL usually appears in their **XML sitemap** before anyone talks about it. Sitemaps are public files that sites publish for search engines, so you can watch them without crawling the site, without logging in and without breaking anything.

The obvious approach is "look at `lastmod` and show me what changed". We tested it on real sites, and it doesn't work reliably. Here's what we found and what to do instead.

## What we measured: 30 SaaS sitemaps (18 Nov 2026)

We pointed our [Sitemap Extractor](https://apify.com/pegton/sitemap-extractor) at the domains of 30 well-known SaaS companies (Notion, Figma, Canva, HubSpot, Shopify, Zapier, Stripe, Slack, Asana, Miro, n8n, Make, Semrush, Ahrefs and others). It read 1,596 sitemap files in about 4 minutes.

| Finding | Sites |
|---|---|
| Have a sitemap at all | **29 of 30** (only intercom.com had none at the usual places) |
| Sitemap listed in robots.txt | 25 of 29. HubSpot, Shopify, Atlassian and Ahrefs only have it at `/sitemap.xml` |
| `lastmod` on ≥ 90 % of URLs | 17 of 29 |
| **No `lastmod` at all** | **6 of 29**: n8n, Webflow, Canva, Slack, Stripe, Ahrefs |
| hreflang language versions | 11 of 29 |
| Median size | 9,575 URLs. Five sites (Figma, Zapier, n8n, Semrush, Gumroad) had more than 300,000 |

### Why `lastmod` alone fails

- **Six sites don't publish it**, so a date filter would show nothing.
- **Some sites stamp everything with today's date.** On Figma, 299,664 of the first 300,000 URLs had a `lastmod` in the last 30 days. On Cloudflare and Shopify, every listed URL did (1,067 and 548). That's the build date, not a content change.
- **Some dates are simply wrong.** One sitemap (Zapier) had a newest `lastmod` of **10 Nov 2027**, a year in the future.

So `lastmod` is a hint, not a signal. The thing you can trust is whether **a URL exists in the sitemap or not**.

## The reliable method: diff the URL set

1. **Baseline:** read all sitemaps of the site (robots.txt first, then `/sitemap.xml`, `/sitemap_index.xml`, `/wp-sitemap.xml`, follow nested indexes and `.gz` files) and save every URL.
2. **Every day:** read them again. URLs that weren't there yesterday are **new pages**, and URLs that disappeared are **removed pages**.
3. **Filter by section** so you don't drown: `/pricing`, `/integrations/`, `/blog/`, `/customers/`, `/vs/`, `/alternatives/`. Comparison and "alternative to X" pages are often the most interesting ones.
4. Use `lastmod` only to sort, never to decide what's new.

It's fast in practice. On BBC's news sitemap (2,184 URLs), a second run 90 seconds after the first found **2 new articles and 1 removed one**, with their headlines from the Google News tags.

## Pitfalls we hit

- **Huge sites.** Some sitemaps list hundreds of thousands of URLs (we stopped at 300,000 per site on purpose). Parse the XML as a stream. Our first cloud test, which loaded each sitemap file into memory at once, failed at 512 MB on a site with over a million URLs. Streaming fixed it.
- **Broken XML.** Some sitemaps are truncated or invalid. Keep the URLs you could read instead of failing the whole site.
- **Blocked sites.** Some sites answer 403 to bots. Respect it, and don't try to get around it.
- **Be polite.** Follow robots.txt and Crawl-delay, use an identifiable user agent, and keep it to a few parallel requests.

## Setting it up

With our [Sitemap Extractor & New Page Monitor](https://apify.com/pegton/sitemap-extractor), the whole thing is one saved task:

```json
{
  "startUrls": ["competitor-a.com", "competitor-b.com", "competitor-c.io"],
  "includePatterns": ["/pricing", "/integrations/", "/blog/", "/vs/"],
  "onlyNewUrls": true
}
```

Schedule it daily. The first run stores the baseline, and later runs return only new URLs plus a site row with `removedSinceLastRun`. URLs cost $0.10 per 1,000 and unchanged URLs are free, so 10 competitors with ~30 new pages a day cost about $0.013 a day.

**See what changed on a page, not just that it's new:** send the new URLs to our [Website Screenshot & Change Monitor](https://apify.com/pegton/website-screenshot-monitor), or use the free [Website Change Monitor n8n template](https://pegton.gumroad.com/l/website-change-monitor-n8n), which marks changes in red and learns which page areas are just noise. Background: [why a 1 % threshold alerts on most pages](/en/n8n-website-change-monitor-visual-diff/).

**Track competitors' hiring too:** a new team often shows up as job postings before it shows up as pages. See [new-job alerts and hiring signals](/en/n8n-new-job-alerts-hiring-signals/).

More guides: [n8n templates & guides](/en/)

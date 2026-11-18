---
title: "How to track your app's App Store keyword rankings with Apple's official API"
description: "A practical guide to iOS keyword and top-chart rank tracking without a paid ASO tool: which Apple endpoints to use, rate limits, and a stale-data problem we measured on 18 Nov 2026 that causes false alerts."
date: 2026-11-18
lang: en
tags: [app store, ASO, keyword ranking, ios, top charts, rank tracking]
layout: post
permalink: /en/app-store-keyword-rank-tracking/
---

Paid ASO tools charge a monthly subscription, but most of what a small app team needs from them is two numbers: **where does my app rank for my keywords, and did a competitor change something?** Apple publishes everything you need for both through public, documented endpoints. This guide shows which ones, what they return, and one trap that produces false "competitor changed" alerts if you don't handle it.

## The three Apple endpoints

| What you need | Endpoint | Notes |
|---|---|---|
| Search results for a keyword | `itunes.apple.com/search?term=…&country=us&entity=software&limit=200` | Up to 200 apps, in ranked order, with full details |
| Details for known apps | `itunes.apple.com/lookup?id=…,…&country=us` | Many IDs or bundle IDs in one call |
| Top charts | `itunes.apple.com/{country}/rss/topfreeapplications/limit=100/genre=6007/json` | Top Free, Paid, Grossing, New, for iPhone, iPad and Mac, per category |

None needs an API key or a login. Apple asks for **about 20 calls per minute** to the Search and Lookup API, so pace your requests. Lookups accept many IDs at once, so 1,000 apps need only a handful of calls.

## What we measured (18 Nov 2026)

We ran two keywords and four charts in the US and Germany and tracked three apps:

- "habit tracker" returned **192 apps in the US and 183 in Germany**, "budget app" 193 and 181. So a search check covers about 180–200 positions, which is deeper than most people look.
- **Streaks** ranked **#14** for "habit tracker" in the US and **#32** in Germany. Ranks differ a lot between storefronts, so track each country separately.
- **Notion** was **#56** in US Top Free Productivity and **#55** in Germany. Duolingo was #23 in US Top Grossing overall and #7 in Germany.
- The whole run (427 apps with full details plus 36 rank checks) took **54 seconds** with 18 API calls and 8 chart requests. No call needed a retry.

## The trap: Apple sometimes serves older data

We ran the same input again three minutes later and compared every app. Without filtering, almost every Google app looked "changed":

- YouTube's rating count went **down** by 3,625 (49,939,952 → 49,936,327).
- YouTube Music's version went **backwards** from 9.46.5 to 9.46.

Nothing really changed. Apple's API answers from several caches, and some of them are a few minutes or hours behind. If your monitor alerts on every difference, you'll get noise every day.

What worked for us:

1. **Never report a version downgrade.** Compare versions numerically and keep the higher one.
2. **Ignore rating changes below 0.01**, and report the rating count as a delta (`ratingCountDelta`) instead of as a "change".
3. Alert only on fields that matter: price, version, rating, name, category, minimum iOS version and developer.

With these rules, the second run reported **0 changes across 427 apps**, which was correct.

## Search position ≠ exactly what your phone shows

The Search API ranks apps the way the App Store does, but your phone's results can differ slightly because of Search Ads, personalisation and the device. For tracking it doesn't matter much: if you use the same source every day, a move from #16 to #14 is real. Just don't promise a client "#1 on every iPhone".

## A minimal daily setup

1. Pick 5–20 keywords per country that real users type (start with the words in your subtitle and your competitors' names).
2. List your app and 2–4 competitors.
3. Every morning, run each keyword with `limit=200` and record each tracked app's position (or "not in top 200").
4. Fetch the top charts for your category and record positions there too.
5. Store yesterday's values and output only the differences, using the rules above.

Our [App Store Scraper & ASO Rank Tracker](https://apify.com/pegton/app-store-scraper) on Apify does exactly this: `trackApps` gives one rank row per app, keyword and country with `previousRank` and `rankChange`, and `onlyChanged` skips unchanged apps so they aren't charged. A rank-only check costs $0.001, so 3 apps × 5 lists × 2 countries ≈ $0.03 a day. App details cost $0.40 per 1,000.

## Add reviews to the picture

Rank drops often follow a bad update. Our [App Store & Google Play Reviews Scraper](https://apify.com/pegton/app-review-monitor) fetches new reviews only, and the free [App Review Alerts n8n template](https://pegton.gumroad.com/l/app-review-alerts-n8n) sends them to Slack or e-mail with a topic summary.

More guides: [n8n templates & guides](/en/) · [Free website change monitor](/en/n8n-website-change-monitor-visual-diff/)

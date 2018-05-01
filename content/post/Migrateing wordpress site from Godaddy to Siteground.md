---
title: "Migrating Wordpress site from GoDaddy to Siteground"
date: 2018-04-05
subtitle: ""
tags: ["Rails", "API"]
draft: false
---

<!--more-->

I'm obsessed with making sites load faster, and for good reason! It's no secret that faster page load times help with SEO, conversions, and tons of other metrics.

- [Why Performance Matters- Google](https://developers.google.com/web/fundamentals/performance/why-performance-matters/)
- [Speed Is A Killer – Why Decreasing Page Load Time Can Drastically Increase Conversions](https://blog.kissmetrics.com/speed-is-a-killer/)

In this post I'll outline how I migrated a client's website from GoDaddy to Siteground and experienced .......


# Siteground vs GoDaddy
[It's](https://www.forbes.com/sites/kellyclay/2012/09/10/5-reasons-you-should-leave-godaddy-and-how/) [no](https://www.reddit.com/r/sysadmin/comments/4txq5q/why_does_rsysadmin_hate_godaddy/) [secret](https://www.quora.com/Is-GoDaddy-a-good-choice-for-web-hosting) that GoDaddy doesn't have the best reputation. You can find an endless trail of posts online detailing their issues with dated infrastructure, lackluster security, and bad customer service.

Some things that stood out to me right away were:

- Siteground consistently score better than GoDaddy in benchmarks [1](http://www.onlinemediamasters.com/siteground-vs-godaddy-wordpress/) [2](https://inlinehostblogger.com/siteground-vs-godaddy) [3](https://www.wpsitecare.com/performance-of-7-top-wordpress-hosting-companies-compared/)
- Siteground consistently score better than GoDaddy in benchmarks customer reviews Trustpilot: [G](https://www.trustpilot.com/review/www.godaddy.com)v[S](https://www.trustpilot.com/review/www.siteground.com) WebHostingGeeks: [G](https://webhostinggeeks.com/providers/godaddy)v[S](https://webhostinggeeks.com/providers/siteground) HostAdvice: [G](https://hostadvice.com/hosting-company/godaddy-reviews/)v[S](https://hostadvice.com/hosting-company/siteground-reviews/).
- Siteground makes a [strong effort](https://www.siteground.com/speed) to keep their infrastructure up to date and speedy. GoDaddy... not so much.

One strong example of this:
In GoDaddy's [own words](Benchmarks for PHP 7 consistently show speeds twice as fast as PHP 5.6.) "Benchmarks for PHP 7 consistently show speeds twice as fast as PHP 5.6". Yet they were **two years** behind [Siteground](https://www.siteground.com/blog/php-7-with-opcache/) to adapt to the superior technology. Given GoDaddy's statement, that's two years of your site running twice as slow for sticking with them.

# Before & After Results
Before:
https://gtmetrix.com/reports/livingleanprogram.com/Y0J7utMh
https://www.webpagetest.org/result/180406_W6_80a1488d2fe00c35e25116611e96e3dc/#run2

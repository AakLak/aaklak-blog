---
title: "How Ditching GoDaddy Improved a Client's Sitespeed by 45%"
date: 2018-04-05
subtitle: ""
tags: ["Rails", "API"]
draft: false
---

I'm **obsessed** with making sites load faster, and for good reason! It's no secret that faster page load times improve SEO, conversion rate, and tons of other desirable metrics.
<!--more-->

Don't just take my word for it, here's some write-ups from industry leaders on the topic:

- [Why Performance Matters- Google](https://developers.google.com/web/fundamentals/performance/why-performance-matters/)
- [Speed Is A Killer – Why Decreasing Page Load Time Can Drastically Increase Conversions](https://blog.kissmetrics.com/speed-is-a-killer/)
- [The Google Speed Update: Page speed will become a ranking factor in mobile search - Search Engine Land](https://searchengineland.com/google-speed-update-page-speed-will-become-ranking-factor-mobile-search-289904)

In this post I'll outline how & why I migrated a client's website from GoDaddy to [Geekstorage](http://www.geekstorage.com/aff/1054), and experienced 45% faster load times!


# Why *(I Think)* GoDaddy Sucks
## Bad Reputation
[It's](https://www.forbes.com/sites/kellyclay/2012/09/10/5-reasons-you-should-leave-godaddy-and-how/) [no](https://www.reddit.com/r/sysadmin/comments/4txq5q/why_does_rsysadmin_hate_godaddy/) [secret](https://www.quora.com/Is-GoDaddy-a-good-choice-for-web-hosting) that GoDaddy doesn't have the best reputation. You can find an endless trail of posts online detailing their issues with dated infrastructure, overcrowding servers, lackluster security, and bad customer service.

## Slow Adaptation of New Tech
In GoDaddy's [own words](https://www.godaddy.com/garage/php-7-now-available-cpanel-hosting-plans/): "Benchmarks for PHP 7 consistently show speeds twice as fast as PHP 5.6". Yet they were ~**2 years** behind competitors like [Geekstorage](https://www.geekstorage.com/blog/index.php/php-7-0-0-is) & [Siteground](https://www.siteground.com/blog/php-7-with-opcache/) to adapt. Given GoDaddy's own benchmarks, that's 2 years of your site running twice as slow for sticking with them.
Some things that stood out to me right away were:

## Various Other Reasons
It's just way too easy to find evidence of GoDaddy lacking in various key areas. There are plenty of alternatives with superior features and glowing reviews.

I encourage you to do your own research, and see for yourself.

# How I Switched Servers with No Downtime
No need to reinvent the wheel, WPBeginner does a great job [explaining how](https://www.wpbeginner.com/wp-tutorials/how-to-move-wordpress-to-a-new-host-or-server-with-no-downtime/)!

# Results
Simply switching hosts resulted in a **45% faster page load speed!** The main difference was a slow Time To First Byte (TTFB) on Godaddy (2.5+ seconds) which was reduce to about 1 second on GeekStorage.
GeekStorage also allows for PHP 7, and most likely has more optimized & less crowded servers.

## Before
<div class="caption">
<img src="https://ci6.googleusercontent.com/proxy/F7ij41kSyICJF09i6HBL6TbAhbCD2QMW38yLxlTsmQL1wFw1Ub4SeLWG6Z0Wks8s7Ur0M-g=s0-d-e1-ft#https://i.imgur.com/hkdCcb1.png" alt="Yuck Reaction Gif"></div>

## After
<div class="caption">
<img src="https://ci6.googleusercontent.com/proxy/9JPTPo4eFta7WDNiWGP7ELpBglHguAzxMVzihC2UZiVAn71R9p6EYdf49g7o6gMRgA5b8k8=s0-d-e1-ft#https://i.imgur.com/9y3uF5b.png" alt="Yuck Reaction Gif"></div>

# Advice on Picking a Host
- Research benchmarks! I enjoy a site called [Research as a Hobby](https://researchasahobby.com/hosting-performance-contest-september-2018-roundup/) which does an awesome monthly benchmark on various WordPress hosts
- Avoid the [EIG family](https://www.google.com/search?q=researchasahobby&oq=researchasahobby+&aqs=chrome..69i57j69i65j69i60j69i59j69i60l2.3963j1j7&sourceid=chrome&ie=UTF-8) of companies due to their practice of overcrowded servers and lackluster support
- Join WordPress Facebook/Reddit groups to see what hosts people are enthusiastic about

<!-- - Siteground consistently score better than GoDaddy in benchmarks [1](http://www.onlinemediamasters.com/siteground-vs-godaddy-wordpress/) [2](https://inlinehostblogger.com/siteground-vs-godaddy) [3](https://www.wpsitecare.com/performance-of-7-top-wordpress-hosting-companies-compared/)
- Siteground consistently score better than GoDaddy in benchmarks customer reviews Trustpilot: [G](https://www.trustpilot.com/review/www.godaddy.com)v[S](https://www.trustpilot.com/review/www.siteground.com) WebHostingGeeks: [G](https://webhostinggeeks.com/providers/godaddy)v[S](https://webhostinggeeks.com/providers/siteground) HostAdvice: [G](https://hostadvice.com/hosting-company/godaddy-reviews/)v[S](https://hostadvice.com/hosting-company/siteground-reviews/).
- Siteground makes a [strong effort](https://www.siteground.com/speed) to keep their infrastructure up to date and speedy. GoDaddy... not so much. -->


<!--
# Before & After Results
Before:
https://gtmetrix.com/reports/livingleanprogram.com/Y0J7utMh
https://www.webpagetest.org/result/180406_W6_80a1488d2fe00c35e25116611e96e3dc/#run2 -->

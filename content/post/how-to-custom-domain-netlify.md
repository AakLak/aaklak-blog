---
title: "How to Set a Custom Domain Name on Netlify in ~~3~~ 2 Steps"
date: 2017-12-14T03:42:50-08:00
draft: false
---

Quick steps on setting up a custom domain name on [Netlify](https://www.netlify.com/).
<!--more-->
## Info
Site built with [Hugo](https://gohugo.io/) static site generator, [Netlify](https://www.netlify.com/) hosting, and a domain from [Hover]((https://hover.com/OqvbyZbU)).
<sup>Use [this link](https://hover.com/OqvbyZbU) to buy a domain from Hover, and we both get $2 credit!<sup>

## 1. Add a Custom Domain on Netlify
Visit Netlify > Your Site > Settings > Domain Management
Click "Add a custom domain", enter your domain name, and save.
<sup>Tip: Netlify [recommends](https://www.netlify.com/docs/custom-domains/#naked-domains) using the "www prefix".</sup>
![Netlify Domain Management](/img/netlify-domain-management.png)

## 2. Add DNS Record for Netlify
This will point your custom domain name to the content hosted on Netlify.
Netlify will display a "DOC BLOCK" in your domain settings which explains how to point your domain to the Netlify servers. There are various ways to do this, Follow their recommended setup.
![Netlify Domain DNS Settings](https://i.imgur.com/25vkGJ8.png)

On Hover, I did this by adding a CNAME record, which looks like this.
![Hover DNS Record Settings](https://i.imgur.com/5uEwJGI.png)

## 3. Add Netlify Nameservers (Optional, but Awesome)
Using [Netlify's DNS](https://www.netlify.com/blog/2017/03/28/why-you-dont-need-cloudflare-with-netlify/) will allow you to access: SSL for https, Netlify CDN for fast loading, and DDoS protection all for free!

In the Netlify domain settings, click "Use Netlify DNS" (refer to appropriate the image above). Then change the nameservers on your registrar to Netlify's. Here's what that looks like on Hover.
![Hover Edit Nameserver Settings](https://i.imgur.com/hDs3VuW.png)


## You're done!

---
title: "Notes: Deploying Hugo To Netlify"
date: 2017-07-21
subtitle: ""
tags: ["Hugo", "Netlify"]
draft: false
---
Quick steps for deploying [Hugo](https://gohugo.io/) to [Netlify](https://www.netlify.com/).
<!--more-->
## Install Hugo & dependencies
```
$ brew install hugo
$ go get github.com/kardianos/govendor
$ govendor get github.com/gohugoio/hugo
$ go install github.com/gohugoio/hugo
```

## Download a theme
Pick a theme from [Hugo themes](https://themes.gohugo.io/).
Create a `themes` folder in `site` root.<br>
Clone or add theme as submodule into the `themes` folder.<br>
<sup>Netlify requires submodle.</sup>
```
$ cd themes
$ git submodule add https://github.com/halogenica/beautifulhugo.git
$ git submodule init
$ git submodule update
```

## Configure theme
Add `1heme = "theme_name"` to your `config.toml` file in the site root directory.<br>r.
Make sure to read the theme's page on [themes.gohugo.io](themes.gohugo.io) & read their Github for additional theme specific setup info.

## Git it
You know the drill.<br>
Navigate to the folder containing you sire and:
```
$ git init
$ git add .
$ git commit -m 'First commit'
```
Make a new Github repository, add the remote, and push (They tell you exactly how on the create repository page!).
```
$ git remote add origin YOUR_GITHUB_REPOSITORY_URL
$ git push origin master
```

## Hosting
Host on [Netlify](https://netlify.com).<br>
Process is straightforward, visit the site and their onboarding guide explains it excellently!

## Resources:
[Hugo Docs - Install Hugo](https://gohugo.io/getting-started/installing/)
[Hugo Themes](https://themes.gohugo.io/)
[Hugo Docs - Host on Netlify](https://gohugo.io/hosting-and-deployment/hosting-on-netlify/)

---
title: "Notes: Deploying Hugo To Netlify"
date: 2017-07-21
subtitle: ""
tags: ["Hugo", "Netlify"]
---
Deploying [Hugo](https://gohugo.io/) to [Netlify](https://www.netlify.com/) using [Victory Hugo](https://github.com/netlify/victor-hugo) boilerplate.
<!--more-->
## Install Hugo & dependencies
```
$ brew install hugo
$ go get github.com/kardianos/govendor
$ govendor get github.com/gohugoio/hugo
$ go install github.com/gohugoio/hugo
```

## Download or Clone Victor Hugo
```
$ git clone https://github.com/netlify/victor-hugo.git
$ npm insatll
$ npm start
```
Check [Victor Hugo Github](https://github.com/netlify/victor-hugo) for latest readme.

## Download a theme
Create a `themes` folder in `site` root.<br>
Clone or add theme as submodule into the `themes` folder.<br>
<sup>Netlify requires submodle.</sup>
```
$ git submodule add https://github.com/halogenica/beautifulhugo.git
$ git submodule init
$ git submodule update
```
## Configure theme
Copy the `config.toml` file from theme's `exampleSite` folder to `site` root.<br>
Copy any desired layouts from theme folder to `site`.

## Git it
You know the drill.<br>
Head to your `victor-hugo` folder and:
```
$ git init
$ git add .
$ git commit -m 'First commit'
```
Make a new Github repository, add the remote, and push.
```
$ git remote add origin YOUR_GITHUB_REPOSITORY_URL
$ git push origin master
```

## Hosting
Host on [Netlify](https://netlify.com).<br>
Process is straightforward, refer to resources below.


### Resources:
[Hugo Docs - Host on Netlify](https://gohugo.io/hosting-and-deployment/hosting-on-netlify/#readout)<br>
[Netlify - Step-by-Step Guide: Victory Hugo on Netlify](https://www.netlify.com/blog/2016/09/21/a-step-by-step-guide-victor-hugo-on-netlify/)<br>
[Hugo Themes](https://themes.gohugo.io/)
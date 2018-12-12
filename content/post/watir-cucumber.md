---
title: "Web Automation with Watir"
date: 2018-10-04
draft: false
---
How I use Watir to make my life easier.
<!--more-->
# What's Watir?
"An open source Ruby library for automating tests. Watir interacts with a browser the same way people do: clicking links, filling out forms and validating text." - [Watir](http://watir.com/)

# A Quick Example
## Goal
Log into a page and confirm success.

## Setup
{{< highlight bash >}}
$ gem install pry
$ gem install watir
{{< / highlight >}}

Put [chromedriver](http://chromedriver.chromium.org/) in a $PATH directory. *Mine's in /usr/local/bin*


## Workflow
I've found using a [REPL](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop) like [pry](https://pryrepl.org/) helps speed up development. Instead of incrementally adding steps to and rerunning my script, I can use Pry to get real time feedback.

## Starting the Browser & Navigating to a page
Initiate Pry
{{< highlight bash >}}
$ pry
{{< / highlight >}}

Require watir, initiate a browser, and navigate to a page.
{{< highlight ruby >}}
require 'watir'
$b = Watir::Browser.new
=> #<Watir::Browser:0x..f8f7739e910ec7622 url="data:," title="">
$b.goto('http://testing-ground.scraping.pro/login')
{{< / highlight >}}

## Locating & Interacting with Elements
Using the DOM, I realized the user/pass fields had an ID
{{< highlight ruby >}}
username_field = $b.text_field id: 'usr'
password_field = $b.text_field id: 'pwd'
sign_in = $b.button value:'Login'
username_field.set 'admin'
password_field.set '12345'
sign_in.click
{{< / highlight >}}

## Reading the Page
There are various ways to do this. I'll be getting the raw HTML of the page, which is returned as a string, and see if it includes the success element.
{{< highlight ruby >}}
$b.html.include? '<h3 class="success">WELCOME :)</h3>'
=> true
{{< / highlight >}}

You did it!

# My Watir Use Cases
- Bumping forum threads with an 8 hour bump limit - [Gist](https://gist.github.com/AakLak/bf8cc4442f2dac0b3941a10d398764c8)
- Automatic Runescape account creation. Utilizes captcha solving with 2Captcha API & multithreading - [Gist](https://gist.github.com/AakLak/ea93192de7fd661ba086cf2ac609dd50)
- Data scraping & exporting to Google Sheets with text notifications utilizing Twillio API - [Gist](https://gist.github.com/AakLak/af7a06da6175d3aff0afd93fa1354330)

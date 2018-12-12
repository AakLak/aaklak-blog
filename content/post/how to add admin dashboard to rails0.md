---
title: "Testing with Cucumber & Watir"
date: 2018-10-16
subtitle: ""
tags: ["Ruby", "Testing"]
draft: false
---
A quick guide on adding an adnmin dashboard to a Rails app using administrate gem.
<!--more-->

# What's an admin dashboard?
Admin dashboards provide views for your Rails app that allow you to CRUD (create, read update, delete) database records.
Dashboards can be a nice alternative to the console, and great for non-technical endusers.
![Administrate dashboard example](https://cloud.githubusercontent.com/assets/903327/25823003/a5cc6aee-3408-11e7-8bcb-c62bb7addf40.png)

{{< highlight bash >}}
$ rails g administrate:dashboard User
{{< /highlight >}}and add `resources :stats` to my routes. No big deal!
*Note: My user model was created by [devise_token_auth gem](https://github.com/lynndylanhurley/devise_token_auth)*


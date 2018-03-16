---
title: "How to Add an Admin Dashboard to a Rails API using the Administrate Gem"
date: 2018-01-30
subtitle: ""
tags: ["Rails", "API", "administrate", "activeadmin", "rails_admin"]
draft: false
---
A quick guide on adding an adnmin dashboard to a Rails app using administrate gem.
<!--more-->

# What's an admin dashboard?
Admin dashboards provide views for your Rails app that allow you to CRUD (create, read update, delete) database records.
Dashboards can be a nice alternative to the console, and great for non-technical endusers.
![Administrate dashboard example](https://cloud.githubusercontent.com/assets/903327/25823003/a5cc6aee-3408-11e7-8bcb-c62bb7addf40.png)

# Why I Chose the Administrate Gem
After some research, the most discussed gems I noticed were: [activeadmin](https://github.com/activeadmin/activeadmin), [rails_admin](https://github.com/sferik/rails_admin), and [administrate](https://github.com/thoughtbot/administrate). I also found a blog post with the ominous title "[Get Rid of Your Admin Gem](http://rubyjunky.com/get-rid-of-your-admin-gem.html)"... which I took into consideration when making my decision.

The author covers activeadmin & rails_admin specifically, citing the main issues as not following "the rails way" and bloat. This made me think: Something different than MVC (model, view, controller) in my Rails app and a DSL (domain specific language) to learn? Yuck, no thanks!

<div class="caption">
<img src="https://media0.giphy.com/media/3o7TKxZzyBk4IlS7Is/giphy.gif" alt="Yuck Reaction Gif">
<p class="caption-text">My thoughts on DSLs & no MVC</p></div>

But what about administrate? Let's take a look at their Github readme:

>To accomplish these goals, Administrate follows a few guiding principles:
>
- **No DSLs** (domain-specific languages)
- **Support the simplest use cases**, and let the user override defaults with standard tools such as **plain Rails controllers and views**.
- Break up the library into core components and plugins, so **each component stays small and easy to maintain**.

Sounds awesome and covers the concerns mentioned above!

Other things that stood out:

 - Support for Rails 5 API only apps out of the box. No tweaks like inheriting from `ActionController::Base` instead of `::API`.
 - Quick setup with builtin generators
 - We can make use of Devise's `before_action :authenticate_user`, since it follows MVC

It also doesn't hurt that administrate is created by ThoughtBot, the people behind amazing gems like [Factorygirl](https://github.com/thoughtbot/factory_bot) & [Paperclip](https://github.com/thoughtbot/paperclip).

# Setting up Administrate
Github: https://github.com/thoughtbot/administrate
Documentation:  https://administrate-prototype.herokuapp.com/

The process is covered well in the documentation, so to to avoid reinventing the wheel, I'll just cover any anomalies here.

## Tips
- If you're getting an error when running the install generator, like
{{< highlight bash >}}
`derive_class_name': undefined method `class_name' for nil:NilClass (NoMethodError) Did you mean?  class_eval`
{{< /highlight  >}} or
{{< highlight bash >}}`PG::UndefinedTable: ERROR:  relation "users" does not exist (ActiveRecord::StatementInvalid)`
{{< /highlight >}}Make sure that your database associations are all setup properly. I had some questionable associations to adjust.
[Relevant Github Issue](https://github.com/thoughtbot/administrate/issues/375)
- If you're getting `Unable to autoload constant UserDashboard` etc.:
You may need to generate files for additional models manually.
My `user_dashboard.rb` file was created, but empty? I had to manually run
{{< highlight bash >}}
$ rails g administrate:dashboard User
{{< /highlight >}}and add `resources :stats` to my routes. No big deal!
*Note: My user model was created by [devise_token_auth gem](https://github.com/lynndylanhurley/devise_token_auth)*

# Thoughts
Administrate is awesome! Utilizing generators and a MVC setup(**huge plus!**) makes setup & customization simple & familiar.

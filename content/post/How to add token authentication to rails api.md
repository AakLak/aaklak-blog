---
title: "How to Add Token Authentication to a Rails 5 API"
date: 2018-02-17
subtitle: ""
tags: ["Rails", "API", "devise_token_auth", "postman"]
draft: false
---
My notes on authenticating a Rails 5 API
<!--more-->

## Setting up
To start, we'll setup a Rails 5 API only app and a resource called scripts.

{{< highlight bash >}}
$ rails new token-auth-example --api --database=postgresql
$ rails g scaffold Script name:string
$ rake db:migrate
{{< / highlight >}}

## Setting up devise_token_auth
[devise_token_auth](https://github.com/lynndylanhurley/devise_token_auth) helps add "Token based authentication for Rails JSON APIs".
It's compatible with token authentication for multiple frontend libraries such as Angular 2, React, and plain jQuery out of the box.
Add devise_token_auth to your gemfile and bundle.

Basic setup consists of generating the User model and migrating.
Check out the [devise_token_auth](https://github.com/lynndylanhurley/devise_token_auth) and [devise](https://github.com/plataformatec/devise) readmes for additional setup options.
{{< highlight bash >}}
$ rails g devise_token_auth:install User auth
{{< /highlight >}}

Before migrating, remove unneeded attributes in user model & migration ex: confirmable, omniauthable.

{{< highlight bash >}}
$ rake db:migrate
{{< /highlight >}}

Since this is an API only application, there are no views, and we need to tell Devise's to use json instead of flash messages for errors.
This can be done in *config/initializers/devise.rb*
{{< highlight ruby>}}
Devise.setup do |config|
    config.navigational_formats = [ :json ]
end
{{< /highlight >}}

## Protecting Controller Actions with Autnetication
Devise covers different methods to [protect resources with auctication](https://github.com/plataformatec/devise/wiki/How-To:-Define-resource-actions-that-require-authentication-using-routes.rb), we'll be using the before_action.
We'll protect everything except the index and show actions.
*app/controllers/scripts_controller.rb*
{{< highlight ruby>}}
  before_action :authenticate_user!, except: [:index, :show]
{{< /highlight >}}

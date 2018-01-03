---
title: "My Journey Building a Rails 5 API"
date: 2017-12-30T14:05:25-08:00
draft: false
---

Notes & thoughts on building my first API with Rails 5.
<!--more-->

## What am I making?
This app will be used by developers who write scripts which automate repetitive tasks.

Developers will be able to create a profile for their scripts, and use the API to record actions it as automated in the game, ex: runtime, monsters killed, trees cut, fish caught, etc.

This data can be used to track and compare stats between scripts & individual users.

## Getting Started
Going to generate a Rails [API only app](http://edgeguides.rubyonrails.org/api_app.html) using PostgreSQL since I'm hosting on [Heroku](https://www.heroku.com/).

{{< highlight bash >}}
$ rails new rsscriptstats --api --database=postgresql
{{< / highlight >}}*[An Interview with Heroku on Postgres](https://2017.postgresopen.org/blog-heroku/)*


## Setup RSpec
Setup [RSpec](https://github.com/rspec/rspec) before models to utilize its auto-generated model tests.

{{< highlight ruby >}}
group :development, :test do
  gem 'rspec-rails', '~> 3.6'
end
{{< / highlight >}}

{{< highlight bash >}}
$ bundle install
$ rails generate rspec:install
{{< / highlight >}}

## Setup up Active Record
Going to start with a script & commit model.
{{< highlight bash >}}
$ rails g scaffold Script name:string type:string bot_for:string game_for:string
$ rails g scaffold Commit runtime:integer amount:float
{{< / highlight >}}

### Set model associations
Scripts have many commits, commits belong to a script.
{{< highlight ruby >}}
#/app/models/script.rb
class Script < ApplicationRecord
  has_many :commits
end

#/app/models/commit.rb
class Commit < ApplicationRecord
  belongs_to :script
end

#/db/migrate/20171231022832_create_commits.rb
class CreateCommits < ActiveRecord::Migration[5.1]
  def change
    create_table :commits do |t|
      t.belongs_to :script, index: true #Add this line
      t.string :task
      t.float :amount
      t.timestamps
    end
  end
end

{{< / highlight >}}

## Seeding
{{< highlight ruby >}}
test_script = Script.create(name: "YWoodcutter",
              skill: "Woodcutting",
              bot_for: "TRiBot",
              game_for: "Oldschool Runescape 07")


test_commit_1 = Commit.new(task: "Logs Cut",
                              amount: "5")
test_commit_1 = Commit.new(task: "Oak Cut",
                              amount: "10")

test_script.commits << test_commit_1
test_script.commits << test_commit_2
{{</highlight>}}

## Versioning
![Versioning API Comic](https://chriskottom.com/images/versioning-commitstrip.jpg)

Decide on an interface strategy. Different strategies explained here:
[Versioning a Rails API by Chris](https://chriskottom.com/blog/2017/04/versioning-a-rails-api/)

I decided on URL namespaceing since it was covered the most thoroughly online, and  was mentioned in the [Infinifum Rails Handbook.](https://handbook.infinum.co/books/rails/Building%20an%20API)

### Namespacing & Routing
Going to organize my filestructure to better accommodate for versioned API.
![Rails API Version Filestructure](https://i.imgur.com/nBy3ykB.png)

You can namespace controllers by using the module keyword:
[How to Namespace Controllers in Rails](https://devblast.com/b/namespace-controllers-rails)
{{< highlight ruby "hl_lines=1 12">}}
module Api::V1
  class CommitsController < ApplicationController

    def index
      @commits = Commit.all

      render json: @commits
    end

    ...
  end
end
{{</highlight  >}}

Define new route:
{{< highlight ruby "hl_lines=2-3 5-6">}}
Rails.application.routes.draw do
  namespace :api do
    namespace :v1 do
      resources :commits
    end
  end
end
{{</highlight  >}}

Will also need to update the location parameter of the response object in the controller to reflect the new route.
Explained in more detailed on this [Stack Overflow answer](https://stackoverflow.com/questions/23582389/rails-nomethoderror-undefined-method-url-for-controller-i-cant-seem-to-res)
{{< highlight ruby >}}
render json: @commit, status: :created, location: api_v1_commit_url(@commit)
{{</ highlight  >}}

`http://localhost:3000/api/v1/commits/` will now hit your API!

Note:
I had to make sure to permit necessary parameters in the controller before successfully cURL POSTing to the endpoint.


<br><br><br><br><br><br><br><br><br><br><br>
[REST API Versioning - RailsCast](http://railscasts.com/episodes/350-rest-api-versioning?view=asciicast)

[Building the perfect Rails 5 API - Versioning Your API](https://sourcey.com/building-the-prefect-rails-5-api-only-app/#versioning-your-api)

https://stackoverflow.com/questions/3697650/pros-cons-of-storing-serialized-hash-vs-key-value-database-object-in-activereco

https://www.percona.com/blog/2010/01/21/when-should-you-store-serialized-objects-in-the-database/


API Tutorial or Checklist
https://www.airpair.com/ruby-on-rails/posts/building-a-restful-api-in-a-rails-application

Friendly URLs
https://github.com/norman/friendly_id
https://gist.github.com/jcasimir/1209730

Database Design
https://www.startuprocket.com/articles/how-to-design-and-prep-a-ruby-on-rails-model-architecture
https://agilewarrior.wordpress.com/2010/11/09/a-rails-design-process-by-ryan-singer/

https://blog.carbonfive.com/2016/11/16/rails-database-best-practices/
http://sparkmasterflex.com/the-very-best-ruby-on-rails-workflow-ever/

Authentication:
http://www.dailysmarty.com/posts/how-to-use-jwt-authentication-in-a-rails-5-api-app
https://sourcey.com/building-the-prefect-rails-5-api-only-app/#authenticating-your-api
https://scotch.io/tutorials/build-a-restful-json-api-with-rails-5-part-two
https://paweljw.github.io/2017/07/rails-5.1-api-with-vue.js-frontend-part-4-authentication-and-authorization/
https://medium.com/@nick.hartunian/knock-jwt-auth-for-rails-api-create-react-app-6765192e295a
https://www.valentinog.com/blog/devise-token-auth-rails-api/
https://www.valentinog.com/blog/devise-token-auth-rails-api/#Adding_the_Authentication_with_Devise_Token_Auth

Frontend
https://medium.com/devtechtipstricks/build-a-simple-rails-api-server-auth0-jwt-authentication-react-from-scratch-in-30-minutes-or-257cbb2a939a





https://github.com/lynndylanhurley/devise_token_auth
https://github.com/plataformatec/devise/wiki/How-To:-Simple-Token-Authentication-Example


Other:
https://www.theredrooffs.com/blog/2017/services-what-i-wish-i-learned-as-a-new-rails-developer

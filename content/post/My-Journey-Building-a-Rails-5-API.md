---
title: "My Journey Building a Rails 5 API"
date: 2017-12-30T14:05:25-08:00
draft: false
---

Notes & thoughts on building my first API with Rails 5.
<!--more-->

## What am I making?
This app will be used by developers who write scripts which automate repetitive tasks.

Developers will be able to create a profile for their scripts, and use the API to record actions it as automated, ex: runtime, monsters killed, trees cut, fish caught, etc.

This data can be used to track and compare stats between scripts & individual users.

## Getting Started
Going to generate a Rails [API only app](http://edgeguides.rubyonrails.org/api_app.html) using PostgreSQL since I'm hosting on [Heroku](https://www.heroku.com/).

{{< highlight bash >}}
$ rails new rsscriptstats --api --database=postgresql
{{< / highlight >}}*[An Interview with Heroku on Postgres](https://2017.postgresopen.org/blog-heroku/)*


## Setup RSpec
Setup [RSpec](https://github.com/rspec/rspec) before models to utilize its auto-generated tests.

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
### Models & Associations
Going to start with these models:
{{< highlight bash >}}
$ rails g devise_token_auth:install User auth
$ rails g scaffold Script name:string skill:string bot_for:string game_for:string
$ rails g scaffold Commit runtime:integer
$ rails g scaffold Stat task:string amount:float
{{< / highlight >}}

User model generated using [devise_token_auth](https://github.com/lynndylanhurley/devise_token_auth) gem.


I learned:
You can set assocaitions & delete scaffolds via command line scaffold.
rails d scaffold Stat task:string amount:float

### Set model associations
Scripts have many commits, commits belong to a script.
Commits have many stats, stats belong to a commit.

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

Will also need to update the location parameter path in the controller to reflect the new route.
Explained in more detailed on this [Stack Overflow answer](https://stackoverflow.com/questions/23582389/rails-nomethoderror-undefined-method-url-for-controller-i-cant-seem-to-res)
{{< highlight ruby >}}
render json: @commit, status: :created, location: api_v1_commit_url(@commit)
{{</ highlight  >}}

`http://localhost:3000/api/v1/commits/` will now hit your API!

Note:
I had to make sure to permit necessary parameters in the controller before successfully cURL POSTing to the endpoint.

## Adding Authentication
Using [devise_token_auth](ttps://www.valentinog.com/blog/devise-token-auth-rails-api/#Adding_the_Authentication_with_Devise_Token_Auth) gem

Add `devise_token_auth` to gemfile
{{< highlight ruby >}}
gem 'devise_token_auth'
{{< /highlight >}}

Install dependencies
{{< highlight bash >}}
$ bundle
{{< /highlight >}}

Generate a user model
{{< highlight bash >}}
$ rails g devise_token_auth:install User auth
{{< /highlight >}}

If it's an API only app, disable Devise flash messages in `config/initializers/devise.rb`
{{< highlight ruby >}}
Devise.setup do |config|
    config.navigational_formats = [ :json ]
end
{{< /highlight >}}

Migrate
{{< highlight bash >}}
$ rails db:migrate
{{< /highlight >}}

## Accepted Nested Attributes
#rsscriptstats/app/models/commit.rb
Accepted nested parameters in model.
http://api.rubyonrails.org/classes/ActiveRecord/NestedAttributes/ClassMethods.html
{{< highlight ruby "hl_lines=2 4">}}
class Commit < ApplicationRecord
  has_many :stats

  accepts_nested_attributes_for :stats
end
{{< /highlight >}}

Allow the nested resources' params in controller.
{{< highlight ruby >}}
def commit_params
  params.require(:commit).permit(:task, :runtime, :script_id, :user_id, stats_attributes: [:task, :amount, :commit_id])
end
{{< /highlight >}}

## And here's where I stopped writing...
I'll update this with more info when I can.
Below you can find a ton of amazing Rails API resources & guides I found!

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
  Knock
  https://medium.com/@nick.hartunian/knock-jwt-auth-for-rails-api-create-react-app-6765192e295a

devise_token_auth
  https://www.valentinog.com/blog/devise-token-auth-rails-api/#Adding_the_Authentication_with_Devise_Token_Auth

Frontend
https://medium.com/devtechtipstricks/build-a-simple-rails-api-server-auth0-jwt-authentication-react-from-scratch-in-30-minutes-or-257cbb2a939a

Why foreign keys should always be indexed:
https://alexpeattie.com/blog/stop-forgetting-foreign-key-indexes-in-rails-post-migration-script


JWT Token Gems
https://github.com/jwt/ruby-jwt
https://github.com/nsarno/knock
https://github.com/lynndylanhurley/devise_token_auth
https://github.com/plataformatec/devise/wiki/How-To:-Simple-Token-Authentication-Example


Other:
https://www.theredrooffs.com/blog/2017/services-what-i-wish-i-learned-as-a-new-rails-developer

  JSONAPI-Rails
  https://github.com/jsonapi-rb/jsonapi-rails

  Belongs to Devise User
  https://stackoverflow.com/questions/31106980/how-to-create-an-association-between-devise-user-model-and-new-model-in-rails



Problems & Solutions

  https://www.google.com/search?q=rails+api+%22user%22%3A+%5B+%22must+exist%22+%5D&oq=rails+api+%22user%22%3A+%5B+%22must+exist%22+%5D&aqs=chrome..69i57.1332j0j7&sourceid=chrome&ie=UTF-8
  Put user migration before others, change date or other method

  https://stackoverflow.com/questions/48334075/activemodelunknownattributeerror-unknown-attribute-when-seeding-belongs-to-re

  https://stackoverflow.com/questions/46837161/passing-post-request-through-postman-for-has-many-association-in-rails
  https://stackoverflow.com/questions/18486674/rails-model-error-associationtypemismatch-expected-got-string

  https://www.google.com/search?ei=KFpkWtueMoSWjQOT7ZrgBA&q=%22Unpermitted%22+parameter%3A+%3A+%22_attributes%22&oq=%22Unpermitted%22+parameter%3A+%3A+%22_attributes%22&gs_l=psy-ab.3..0i7i30k1j0i7i10i30k1l2.4848.9790.0.10095.4.4.0.0.0.0.91.341.4.4.0....0...1c.1.64.psy-ab..0.4.340...0i8i30k1j0i8i10i30k1j0i10i30k1j0i22i10i30k1j0i22i30k1.0.SMmNrXU8jno


https://stormconsultancy.co.uk/blog/development/tools-plugins/generating-code-coverage-metrics-for-a-ruby-on-rails-project-with-simplecov/

Factorygirl & RSpec
  https://semaphoreci.com/community/tutorials/how-to-test-rails-models-with-rspec

  My Q: https://stackoverflow.com/questions/48499505/devise-factorybot-authentication-for-rspec-post-request
  https://blog.pardner.com/2012/10/how-to-specify-traits-for-model-associations-in-factorygirl/
  https://stackoverflow.com/questions/24570281/what-to-add-to-rspec-valid-attributes-when-it-is-validating-related-models

  https://devhints.io/factory_bot
  https://github.com/thoughtbot/factory_bot/blob/master/GETTING_STARTED.md
  https://github.com/plataformatec/devise/wiki/How-To:-Test-controllers-with-Rails-3-and-4-%28and-RSpec%29
  https://semaphoreci.com/blog/2014/01/14/rails-testing-antipatterns-fixtures-and-factories.html
  https://stackoverflow.com/questions/11002721/authentication-with-devise-in-rspec-tests
  https://gist.github.com/kyletcarlson/6234923
  https://semaphoreci.com/community/tutorials/how-to-test-rails-models-with-rspec
  https://medium.com/@csofiamsousa/testing-covering-and-documenting-ruby-on-rails-apis-26fe4f4c934d

http://www.thegreatcodeadventure.com/better-rails-5-api-controller-tests-with-rspec-shared-examples/


API To Full
 - https://stackoverflow.com/questions/36669981/how-do-you-convert-a-rails-5-api-app-to-a-rails-app-that-can-act-as-both-api-and
 - https://stackoverflow.com/questions/25031985/rails-4-upgrade-jsonparseerror-for-old-sessions

Removing Devise Token Authentication
  - https://stackoverflow.com/questions/6833161/ruby-how-to-uninstall-devise
  - rsscriptstats [remove-devise-token-auth] :> rails destroy devise_token_auth:install User
Running via Spring preloader in process 76916
      remove  config/initializers/devise_token_auth.rb
      remove  db/migrate/20171230080350_devise_token_auth_create_users.rb
     skipped  Concern is already included in the application controller.
     skipped  Routes already exist for User at auth

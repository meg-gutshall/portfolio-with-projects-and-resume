---
layout: post
title: "What's Up With That!?: SINATRA_ENV"
date: 2019-02-15 17:01:16 -0500
permalink: whats-up-with-that-sinatra-env
featured-img: "/static/img/complex-forms-environment.jpg"
categories: [Flatiron School]
tags: [software engineering, Sinatra, environment]
excerpt: <p>If you’re in the Sinatra ActiveRecord section of the Flatiron School, you’ve probably seen this error message a few times “Migrations are pending. Run <code>rake db:migrate SINATRA_ENV=test</code> to resolve the issue.” Well we know we need to run <code>rake db:migrate</code> to create our migration tables, but <code>"SINATRA_ENV"</code>... what’s up with that!? It turns out that <code>"SINATRA_ENV"</code> is much more powerful than you might think and is the key (both literally and figuratively) to making your program run effectively.</p>
---

If you’re in the Sinatra ActiveRecord section of the Flatiron School, you’ve probably seen this error message a few times: “Migrations are pending. Run `rake db:migrate SINATRA_ENV=test` to resolve the issue.” Well we know we need to run `rake db:migrate` to create our migration tables, but `"SINATRA_ENV"`... what’s up with that!?

First let’s go over a few files line by line so we can follow `ENV["SINATRA_ENV"]` on it’s journey through our app. For this, I’m using the [Sinatra Complex Forms Associations Lab](https://github.com/meg-gutshall/sinatra-complex-forms-associations-v-000).

## Rakefile

`"SINATRA_ENV"` is the key to Ruby's `ENV` hash and defines your deployment environment. This is set in your `Rakefile`, where the app's rake tasks are defined.

![Picture of Rakefile](/img/post-images/complex-forms-rakefile.jpg)

### Line 1

```ruby
ENV["SINATRA_ENV"] ||= "development"
```

In most of the labs in this section, this will be the first line of your `Rakefile`. This means that if `"SINATRA_ENV"` doesn’t already have a value, its value will be set equal to `"development"`.

### Line 3

```ruby
require_relative './config/environment'
```

This loads our app’s `environment.rb` file.

### Line 4

```ruby
require 'sinatra/activerecord/rake'
```

This loads Rake tasks from the `sinatra-activerecord` gem. A custom Rake task is defined on Lines 8&ndash;10 which starts a new Pry session.

## Environment.rb

Now let’s check out our `config/environment.rb` file, where we load all of the app's dependencies, from gems to database connections.

![Picture of environment.rb](/img/post-images/complex-forms-environment.jpg)

### Line 1

```ruby
ENV["SINATRA_ENV"] ||= "development"
```

Again, we see our `"SINATRA_ENV"` being defined. In order to maintain DRY code, I removed Line 1 from the `Rakefile` and all my tests for the lab still passed without any failures.

### Lines 3&ndash;4

```ruby
require 'bundler/setup'
Bundler.require(:default, ENV['SINATRA_ENV'])
```

On Lines 3 and 4 we require our gems and dependencies. Line 3 finds our `Gemfile` and makes all the gems contained within (plus their dependencies) available to Ruby by adding them to the load path. In Line 4, we’re requiring all of our gems (`:default` represents all gems since we didn’t create gem groups for this app) as well as our deployment environment hash to be used with the ActiveRecord gem.

### Lines 6&ndash;9

```ruby
ActiveRecord::Base.establish_connection(
  adapter: "sqlite3",
  database: "db/#{ENV['SINATRA_ENV']}.sqlite"
)
```

On Lines 6 through 9, we establish our database connection. Our `adapter` sets the name of the database management system (typed in all lower-case) that we’re using to hold our data and our `database` sets the path to our app’s database. This path includes our deployment environment hash, which is an argument of `Bundle.require` on Line 4 and will change based on our key’s value, pointing to other databases.

### Line 11

```ruby
require_all 'app'
```

This loads all other files nested under `app` to run the program.

## Config.ru

Lastly, let’s look at our `config.ru` file, which details to Rack the environment requirements of the app and then runs the app.

![Picture of config.ru](/img/post-images/complex-forms-config.jpg)

### Line 1

```ruby
require './config/environment'
```

Again, this loads our app’s `environment.rb` file. Notice the difference between this line and Line 3 in our `Rakefile`? Both do the same thing, they just require the `environment.rb` file in different ways.

### Lines 3&ndash;5

```ruby
if ActiveRecord::Migrator.needs_migration?
  raise 'Migrations are pending. Run `rake db:migrate` to resolve the issue.'
end
```

Lines 3 through 5 check to make sure our migrations have been run. If not, the error message on Line 4 is raised.

### Line 8

```ruby
use Rack::MethodOverride
```

`Rack::MethodOverride` is a piece of Sinatra Middleware that intercepts every request run by our application. It will interpret any requests with `name="_method"` by translating the request to whatever is set by the `value` attribute&mdash;normally `PATCH` or `DELETE` for purposes in our Sinatra curriculum. This line must be placed in the `config.ru` file above all controllers in which you want access to the Middleware's functionality.

### Lines 9&ndash;13

```ruby
Dir[File.join(File.dirname(__FILE__), "app/controllers", "*.rb")].collect {|file| File.basename(file).split(".")[0] }.reject {|file| file == "application_controller" }.each do |file|
  string_class_name = file.split('_').collect { |w| w.capitalize }.join
  class_name = Object.const_get(string_class_name)
  use class_name
end
```

In Lines 9 through 13, we're mounting our controllers nested under `app/controllers`. These classes will also be loaded as Middleware. In this case, they'll be `use OwnersController` and `use PetsController`. Notice how `"application_controller"` is rejected? That's because it is called on in Line 14 below.

### Line 14

```ruby
run ApplicationController
```

This line creates an instance of our `ApplicationController` class that can respond to requests from a client. We can only `run` one class and the rest are loaded via `use`.

## Bringing It All Together

So now we know what happens each time the `Rakefile`, the `config/environment.rb` file, and the `config.ru` file is run. We still have to answer the question as to what `ENV["SINATRA_ENV"]` actually is.

`ENV` is a hash-like accessor for Ruby environment variables. It will change depending on what information Ruby needs. The `ENV` accessor holds all types of information for Ruby as well as its corresponding frameworks and gems. In our case, Ruby needs to define a `"SINATRA_ENV"` so this is added as a key to the `ENV`.

When we run `learn` to test our app, this triggers the gem `rspec` which will run our `spec_helper.rb` file.

![Picture of spec_helper.rb](/img/post-images/complex-forms-spec-helper.jpg)

### Line 1

```ruby
ENV["SINATRA_ENV"] = "test"
```

As we can see on Line 1, our `ENV["SINATRA_ENV"]` is being set equal to `"test"` so that when we go to our `environment.rb` file as directed on Line 3, `ENV["SINATRA_ENV"]` is already assigned a value and, therefore, does not take on the value of `"development"`. This means that on Line 4 of our `environment.rb` file, the deployment environment hash dependency has the value of `"test"` and on Line 8, we are establishing a connection with a different database entirely&mdash;`db/test.sqlite`! This database contains seed data to match the expected test outputs.

Lines 4 through 6 require gems related to testing.

### Lines 8&ndash;10

```ruby
if ActiveRecord::Migrator.needs_migration?
  raise 'Migrations are pending. Run `rake db:migrate SINATRA_ENV=test` to resolve the issue.'
end
```

Lines 8 through 10 check to make sure our migrations have been run. If not, the error message on Line 9 is raised... and now we've come full circle. The app will not function unless we've run our migrations first because we won't have any information to display or manipulate.

The rest of our `spec_helper.rb` file configures the app for testing and then runs our `config.ru` file.

## Shotgun

There's one last difference I'd like to point out. In the Sinatra section, we learn about a trusty little gem called `shotgun` which allows you to start your Rack-based web app with an automatic code reloading feature. It starts up a Rack server and listens for any requests so when you hit the browser's refresh button, you can see changes made to your code that have been saved. All you have to do is type `shotgun` in your terminal to start up a new session.

Starting a new `shotgun` session is like starting a new `rackup` session in that the first thing our app loads is our `config.ru` file, which on the very first line, requires our `environment.rb` file. At this point, `ENV["SINATRA_ENV"]` will be set equal to `"development"` and the rest of our `environment.rb` file will execute, bringing us back to `config.ru`. Again, we have the chance of raising an error message on Line 4 of our `config.ru` file if we have not already run our migrations. However, this message simply says "Run `rake db:migrate` to resolve the issue" instead of "Run `rake db:migrate SINATRA_ENV=test` to resolve the issue" Why is that? It's because of the line of code `ENV["SINATRA_ENV"] ||= "development"`, which sets `"development"` as our default value for the `"SINATRA_ENV"` key. Since that's the default, we don't have to specify another `"SINATRA_ENV"` in our error message.

## Conclusion

`ENV["SINATRA_ENV"]` started out as a mixed-up mess for me but after I lot of research and deep-diving into my code files, I came to understand it's purpose throughout my programs. Hopefully this post helped you understand it a little bit better as well.

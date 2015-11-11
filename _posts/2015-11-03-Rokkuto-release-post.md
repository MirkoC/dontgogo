---
layout: post
title: "Rokkuto - my first production ready web service"
description: "Rokkuto release blog post"
tags: [Rokkuto, web service, release, API]
image:
  feature:
  credit:
  creditlink:
---


### What is Rokkuto?



### What does Rokkuto solve?

For example, lets say that you have an web application for grocery lists where users can add a grocery lists and check them out on their phones when they're out doing groceries. For the first version of the application you decided that each user can see only his own grocery lists, but not to assign other members to it or share grocery lists with them. You have other projects to attend to and simply you don't have time to implement fairly complex database model for assigning other users to grocery lists they didn't create. So, to satisfy your users and yourself you could implement it with the Rokkuto. Simple two calls to Rokkuto API (plus few adjustments in your back-end) can solve your problem.

Not only you've solved your problem with sharing but Rokkuto has an mailer solution to notify your users that somebody shared a grocery list with them.

Other feature is that even if somebody wants to share a grocery list with their grandmother (who, of course, doesn't have an account on yours grocery lists application) he can. You'll just need to enable it in your application.

Besides sharing objects, current Rokkuto version isn't capable for doing much more, but it has a great potential.


### How is build?


<a href="https://github.com/MirkoC" target="_blank">GitHub repo</a>

<a href="" target="_blank">Application</a>

API: <a>www.rokkuto.com/api</a>




Rokkuto is build with Rails 4.2.4. Need to mention gems (and services) I've used are <a href="https://github.com/apotonick/roar-rails" target="_blank">roar-rails</a>, <a href="https://github.com/mperham/sidekiq" target="_blank">sidekiq</a>, <a href="https://github.com/wildbit/postmark-rails" target="_blank">postmark-rails</a>, <a href="https://github.com/richhollis/swagger-docs" target="_blank">swagger-docs</a> and <a href="https://github.com/vmg/redcarpet" target="_blank">redcarpet</a>.

##### Roar

For easier data modeling I've used representers (roar-rails). When you use representers it's easy to render json response, save or update object and similar things.

For example I want to render json containing all objects of some class. I can easily do something like this

``` ruby
def index
  render json: Object.all
end
```

without worrying that I'll expose something I don't want to expose through API. All I have to do is set representer to look like this and I will not expose `private_key` through API.

``` ruby
module AuthObjectRepresenter
  include Roar::JSON

  property :id
  property :name
  property :public_key
  property :private_key, getter: nil
end
```

But when I need to save or update my object using representer, `private_key` will be saved. That is the other great thing about representers. When I need to save the object I'll do something like this

``` ruby
def create
  @object = consume! Object.new
end
```

no need for meddling with params, roar does everything for me.

If you like Rails magic then roar representers are the thing for you and be shure to check it out.

##### Sidekiq

Sidekiq is easy to setup, reliable processing for Ruby.
I use sidekiq for my mailer jobs. I don't want to wait for all mails to be sent before server is ready to render json. If you haven't already, you must check out sidekiq.


### Future development

For now Rokkuto has fairly limited usage opportunities. Everything shared via Rokkuto has a public link. That is the main disadvantage of Rokkuto, but it's also a feature.

Next logical step would be adding private sharing. That would require some sort of authentication on the client side or Rokkuto.

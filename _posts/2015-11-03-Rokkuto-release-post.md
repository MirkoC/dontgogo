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


### Why Rokkuto?

Rokkuto is my first production ready web service. The idea for it came from my colleagues. Feel free to check out their blogs (<a href="http://pltconfusion.com/" target="_blank">PLTConfusion</a>, <a href="http://retroaktive.me/" target="_blank">Retro Aktive</a>).

The idea behind Rokkuto is to create a useful open source project for my github repo. It evolved over time into something that has a great potential. Rokkuto is an authentication service with potential to grow into an authorization service that can be used for all our projects. If, in the meantime. it serves well in the open source community, that would be great.


### What problem does Rokkuto solve?

For example, lets say that you have a web application where users can add a grocery list and check it out on their phones while they're out doing groceries. For the first version of the application you decided that each user can see only his own grocery lists, but not to assign other members to it or share grocery lists with them. You have other projects to attend to and simply don't have the time to implement a fairly complex database model for assigning other users to grocery lists they didn't create on their own. So, to satisfy your users and yourself, you could implement it with Rokkuto. Simple two calls to Rokkuto API (plus few adjustments in your back-end) can solve your problem.

Not only have you solved your problem with sharing, but Rokkuto has a mailer solution to notify your users that somebody shared a grocery list with them.

An other feature is that even if somebody wants to share a grocery list with their grandmother, he can. (Of course, she doesn't have an account on yours grocery list application) You'll just need to enable it in your application.

Besides sharing objects, the current Rokkuto version isn't capable for doing much more, but it has a great potential.


### How is it built?

<a href="https://github.com/MirkoC" target="_blank"><i class="fa fa-github"></i></a> <a href="http://rokkuto.floatingpoint.io/" target="_blank"><i class="fa fa-bomb"></i></a> API: <a href="http://rokkuto.floatingpoint.io/api" title="http://rokkuto.floatingpoint.io/api" target="_blank"><i class="fa fa-anchor"></i></a>




Rokkuto is built with Rails 4.2.4. Need to mention gems (and services) I've used are <a href="https://github.com/apotonick/roar-rails" target="_blank">roar-rails</a>, <a href="https://github.com/mperham/sidekiq" target="_blank">sidekiq</a>, <a href="https://github.com/wildbit/postmark-rails" target="_blank">postmark-rails</a>, <a href="https://github.com/richhollis/swagger-docs" target="_blank">swagger-docs</a> and <a href="https://github.com/vmg/redcarpet" target="_blank">redcarpet</a>.

##### Roar

For easier data modeling I've used representers (roar-rails). When you use representers it's easy to render a json response, save or update an object and similar things.

For example I want to render json containing all objects of some class. I can easily do something like this

``` ruby
def index
  render json: Object.all
end
```

without worrying that I'll expose something I don't want to expose through API. All I have to do is set the representer to look like this and I will not expose `private_key` through API.

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

No need for meddling with params, roar does everything for me.

If you like Rails magic then roar representers are the thing for you and be sure to check it out.

##### Sidekiq

Sidekiq is easy to setup, a reliable background processing for Ruby.
I use sidekiq for my mailer jobs. I don't want to wait for all mails to be sent before the server is ready to render json. If you haven't already, you must check out sidekiq.


### Future development

For now Rokkuto has fairly limited usage opportunities. Everything shared via Rokkuto has a public link. That is the main disadvantage of Rokkuto, but it's also a good feature.

The next logical step would be adding private sharing. That would require some sort of authentication on the client side or Rokkuto. Rokkuto could have users and those users could share privately between each other.

An other big potential for Rokkuto is that it can evolve in authorization service for all our (my company) projects and applications. Same as Google has one login for all their products.

It would significantly facilitate the development of our in house applications and possibly those of other users from the open source world.

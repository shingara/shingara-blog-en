---
layout: post
title: "Moped the new mongoDB ruby driver"
date: 2012-05-07 09:38
comments: true
categories:
  - ruby
  - mongodb
  - mongoid
---

Since December 2012, [Bernerd Schaefer](https://github.com/bernerdschaefer) start to create a new MongoDB ruby driver.
This new driver is not supported by 10Gen (MongoDB creator) directly.
The official MongoDB ruby driver is the gem [mongo-ruby-driver](https://github.com/mongodb/mongo-ruby-driver).

# Goal of this gem

This gem will be created after some frustration from the Mongoid Core
developer. This team try to do some proposal to change the internal
software design on MongoDB Ruby driver. But all of this change will be
refused.

## Thread-safe

One of goal of this gem is to have a gem more thread-safe than official
MongoDB ruby driver. This official gem is sometimes consider like not
full fully thread-safe like report by [Mike Perham](https://github.com/mperham/sidekiq/wiki/Problems-and-Troubleshooting).
It's a real problem actually, when the ruby community discover the
thread. With Moped we can use [sidekiq](http://mperham.github.com/sidekiq/).

## No more extension

If you want use the MongoDB official driver, you need install another
gem [bson_ext](http://rubygems.org/gems/bson_ext). Without it, you have
some performance penalties. This gem is a C extension. So to be
compatible with JRuby this gem has 2 versions. One in C and other in
Java. It's harder to maintain it.

With Moped, no more extension needed. All is do in pure ruby. Some
benchmark show this moped's bson version is even faster than the C extension.

The only case where moped is slower than bson_ext is the ObjectId
generation, like report on [Mongoid mailing-list](https://groups.google.com/d/topic/mongoid/87IdIKO8-VM/discussion).
But Moped is new. this can be improved in future.

## Cleanest API

The Moped API is really clean and simple. It's depend of developer using
it. But to me it's a better API. It's really more ruby way.

## Better Replicat Set management

One of big feature of MongoDB is the ReplicatSet. With Moped, his
management is better. There are no exception raise when a node is down.
the switch of node is immediately doing by moped.

No more needed to list all of your node. Moped do it to you with MongoDB
mechanism. You can even add some new node in your Replicat without
restart of your application.

# Limitation

Moped has some limitation not report on Mongo-ruby-driver

## Ruby 1.9 only

Moped works only with Ruby > 1.9. This choice is to avoid some problem
with the 1.8 compatibilities. The Socket on Ruby 1.8 series is not the
best by example.

## No GridFS support

GridFS is not support on Moped. This choice is to limit the moped's code core. In future, this feature can be add in some moped extension.

# Mongoid 3 integration

Mongoid 3 now use only this MongoDB ruby driver. Moped is a new
dependencies of Mongoid and mongo-ruby-driver is not anymore a
dependencies. If you want try this new driver you can test on Mongoid 3.

Warning: Mongoid 3 is not released actually. If you want test it, you
need use the Master branch on Mongoid.

```ruby
gem 'mongoid', :git => 'git://github.com/mongoid/mongoid'
```

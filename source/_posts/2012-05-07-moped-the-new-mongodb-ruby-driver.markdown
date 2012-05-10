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

In December 2011, [Bernerd Schaefer](https://github.com/bernerdschaefer) created a new MongoDB ruby driver.
This new driver is not supported by 10Gen (MongoDB creator) directly.
The official MongoDB ruby driver is the gem [mongo-ruby-driver](https://github.com/mongodb/mongo-ruby-driver).

# Goal of this gem

This gem will be created after some frustration from the Mongoid Core
developer
The team proposed to change the internal software design on MongoDB Ruby driver but all changes were refused.

## Thread-safe

One of this gem's goals was to have a more thread-safe gem than the official MongoDB ruby driver.
This official gem is sometimes considered like not 100% thread-safe like reported [Mike Perham](https://github.com/mperham/sidekiq/wiki/Problems-and-Troubleshooting).
Since the ruby community discovered again the thread, it hasn't stopped of being a real problem.
With Moped we can use [sidekiq](http://mperham.github.com/sidekiq/).

## No more extension

If you want to use the MongoDB official driver, you will need to install another
gem [bson_ext](http://rubygems.org/gems/bson_ext).
Without it, you will have some performance penalties.
As this gem is a C extension, this gem has 2 versions in order to be compatible with JRuby.
One in C and the other in Java. It's therefore harder to maintain.

With Moped, no more extension needed.
Everything is done in pure ruby.
A benchmark shows that this moped's bson version is even faster than the C extension.

The only case where moped is slower than bson_ext is the ObjectId generation, like reported on [Mongoid mailing-list](https://groups.google.com/d/topic/mongoid/87IdIKO8-VM/discussion).
But, after all, Moped is new.
It can be improved in the future.

## Cleanest API

The Moped API is really clean and simple.
It depends on the developers using it.
But, to me, it's a good API because it's really more "ruby way".

## Better Replicat Set management

One of big feature of MongoDB is the ReplicatSet.
With Moped, the ReplicatSet management is even better.
There are no exception raise when a node is down.
The switch off of node is immediately done by Moped.

No need to list all of your node.
Moped does it automaticly with the MongoDB mechanism.
You can even add new nodes in your Replicat without restarting your application.

# Limitation

Moped has some limitation in front of Mongo-ruby-driver.

## Ruby 1.9 only

Moped works only with Ruby > 1.9. in order to avoid troubles with the 1.8 compatibilities.
For instance, the Socket on Ruby 1.8 series is not the best.

## No GridFS support

GridFS is not supported on Moped.
This choice has been made to limit the size of the Moped code core.
In the future, this feature may be added in a Moped extension.

# Mongoid 3 integration

Mongoid 3 now uses only MongoDB ruby driver.
Moped is a new dependency of Mongoid and mongo-ruby-driver is not anymore a dependencie.
If you want to try this new driver you can test it on Mongoid 3.

Warning: Mongoid 3 is still not released.
If you want to test it, you will need to use the Master branch on Mongoid.

```ruby
gem 'mongoid', :git => 'git://github.com/mongoid/mongoid'
```

[French Translation](http://blog.shingara.fr/moped-le-nouveau-driver-mongodb-pour-ruby.html)

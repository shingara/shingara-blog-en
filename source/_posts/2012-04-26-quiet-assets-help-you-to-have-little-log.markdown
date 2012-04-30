---
layout: post
title: "Quiet assets help you to have little log"
date: 2012-04-26 09:39
comments: true
categories:
  - rails
  - assets_pipeline
  - gem
---

One week ago, I discover a new gem really helpfull, [quiet_assets](https://github.com/evrone/quiet_assets).

This gem is not a big deal, but it help a lot when you develop a Rails
application ( > 3.1 ) with 'assets_pipeline'. If you have a lot of
assets you have already see in your log a lot of information about
requests doing on this assets. You can see log like :

```
Started GET "/assets/application.js?body=1" for 127.0.0.1 at 2012-02-13 13:24:04 +0400
Served asset /application.js - 304 Not Modified (8ms)
```

After, when you want see the log really interresting about the request
you do, you need up on your log. It's really annoying. Our log file is
really bloat by this logs.

But [quiet_assets](https://github.com/evrone/quiet_assets) is here to
help you in this mess. After adding this gem in your Gemfile.

```
gem 'quiet_assets', :group => :development
```

You can discover your log clean and light like before assets_pipeline. No more logs about your assets
request. Just your log you really need.

Some gem can really be usefull even if it's little.

Since rails 3.2.0, there are an option to define the logger of
Sprockets. All informations is on this [issue 2639](https://github.com/rails/rails/issues/2639).
But quiet_assets is allways usefull. When you define your sprocket's log to
false you have allways logs from actionpack. You have less log, but
allways too much.

[French Translation](http://blog.shingara.fr/quiet-assets-le-limiteur-de-log.html)

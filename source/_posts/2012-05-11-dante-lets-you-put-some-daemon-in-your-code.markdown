---
layout: post
title: "Dante lets you put some daemon in your code"
date: 2012-05-11 09:54
comments: true
categories:
  - dante
  - gem
  - ruby
  - daemon
---
Few days ago, I needed to convert a ruby script into daemon.

In my deployment toolkit, I use monitrc to monitor my scripts. Monit
checks a pidfile to know if the process is running or not.

So, I looked the best ruby solution to easily to transform this script in deamon
generating its own pidfile.

The most popular project in ruby to convert a ruby script into daemon is
[daemons](http://daemons.rubyforge.org/). I've tried several times in the past
and never became fan. It's too complicated and doesn't generate pidfile.
You have to manage everything yourself.

The second big project about managing daemon in the ruby community is
[daemon-kit](https://github.com/kennethkalmer/daemon-kit). I've tried it
several times without success. To me, it's way too big and not
flexible enough to use on a simple daemon script.

Therefore, I decided to see if the ruby community had a new
gem for this purpose. I discovered [dante](https://github.com/bazaarlabs/dante).
This project was exactly what I really needed. It really gets the job done.

By default, Dante manages few options in commandline. These options
are all what you really need. No more useless stuff.

Default options available :

```
    -p, --port PORT           Specify port
                              (default: )
    -P, --pid FILE            save PID in FILE when using -d option.
                              (default: /var/run/scheduler.pid)
    -d, --daemon              Daemonize mode
    -l, --log FILE            Logfile for output
    -k, --kill [PORT]         Kill specified running daemons - leave
blank to kill all.
    -u, --user USER           User to run as
    -G, --group GROUP         Group to run as
    -?, --help                Display this usage information.
```

You can define the PID, the log file, the user and group launching this
daemon (if launched in root). The only argument which is not essential to me is
the PORT. But this gem was created to launch rack applications. So
it's really needed in this case.

An other great feature is the option extension. You can add some new
options. Dante manage to you all options pass on your script.

To use it, it's really simple. You just put `Dante.run` and put your
code in a block. :

```ruby
Dante.run('myapp') do |opts|
  myapp.run
end
```

I am really impressed by this gem by the [gomiso](http://gomiso.com)
company. Thanks for creating such a good gem.

[Traduction Française](http://blog.shingara.fr/dante-pour-mettre-du-daemon-dans-son-code.html)
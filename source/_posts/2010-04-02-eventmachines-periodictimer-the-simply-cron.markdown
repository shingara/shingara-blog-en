--- 
layout: post
title: EventMachine's PeriodicTimer the simply cron
typo_id: 23
---
[EventMachine](http://rubyeventmachine.com/) is one of the best Ruby libraries. But it's also the most unknown and unused.

I started to look at Event Machine and where I could use it. The first application I found was the [PeriodicTimer](http://eventmachine.rubyforge.org/EventMachine/PeriodicTimer.html).

This feature aims to replace the cronjob.

Let me explain : a cronjob is really a great tool. We can precisely define the time where task is executed. Usually, this task executes precisely at a given time… But when a task is executed too soon, using a cronjob can become awful.

Cron doesn't have any queue system, therefore there can be an increase of the number of tasks running at same time. Your server might become overloaded and in that case you'd have to restart it. This situation would happen only if your task is created to be executed more times than your frequency of execution.

The solution can be to have a little application like [jobq](http://forge.bearstech.com/trac/wiki/JobQueue) to manage your queue system. But in many cases you don't really need a queue system, so it would be a little bit like overkill.

The PeriodicTime's advantage is to execute after a time define precisely. There is no management of time: only the elapse time needs to be managed. The task is sent after X seconds after the end of the previous task and when there are no tasks running anymore. There might be some little variations in the time the task will execute, but it's not really a problem. The most important is that the task is executed regularly (e.g. when fetching statistics).

Below is [a sample of code]http://gist.github.com/345000)  using PeriodicTimer.

<typo:code lang="ruby">
require 'eventmachine'
require 'timeout'
EventMachine.run {
 EventMachine::PeriodicTimer.new(10) do
   puts "#{Time.now} : I am 10"
   sleep 10
 end

 EventMachine::PeriodicTimer.new(1) do
   puts Time.now
 end
}
</typo:code>

And the output is after 50 second of execution.

<typo:code lang="plain">
$ ruby em_periodic.rb
Fri Mar 26 16:07:39 +0100 2010
Fri Mar 26 16:07:40 +0100 2010
Fri Mar 26 16:07:41 +0100 2010
Fri Mar 26 16:07:42 +0100 2010
Fri Mar 26 16:07:43 +0100 2010
Fri Mar 26 16:07:44 +0100 2010
Fri Mar 26 16:07:45 +0100 2010
Fri Mar 26 16:07:46 +0100 2010
Fri Mar 26 16:07:47 +0100 2010
Fri Mar 26 16:07:48 +0100 2010 : I am 10
Fri Mar 26 16:07:58 +0100 2010
Fri Mar 26 16:07:59 +0100 2010
Fri Mar 26 16:08:00 +0100 2010
Fri Mar 26 16:08:01 +0100 2010
Fri Mar 26 16:08:02 +0100 2010
Fri Mar 26 16:08:03 +0100 2010
Fri Mar 26 16:08:04 +0100 2010
Fri Mar 26 16:08:05 +0100 2010
Fri Mar 26 16:08:06 +0100 2010
Fri Mar 26 16:08:08 +0100 2010
Fri Mar 26 16:08:08 +0100 2010 : I am 10
Fri Mar 26 16:08:18 +0100 2010
Fri Mar 26 16:08:19 +0100 2010
Fri Mar 26 16:08:20 +0100 2010
Fri Mar 26 16:08:21 +0100 2010
Fri Mar 26 16:08:22 +0100 2010
Fri Mar 26 16:08:23 +0100 2010
Fri Mar 26 16:08:25 +0100 2010
Fri Mar 26 16:08:26 +0100 2010
Fri Mar 26 16:08:27 +0100 2010
Fri Mar 26 16:08:28 +0100 2010
Fri Mar 26 16:08:28 +0100 2010 : I am 10
</typo:code>

We can see that the task is executed each second. But the other task executing after 10 seconds, runs and locks the thread. So the first task (executing each second) is executed only after the end of second one. After that, each second the task restarts. and locks again after 10 seconds.

This is really the great strength of PeriodicTimer. We can easily make a suite of tasks who will never execute at the same time. Lately I'm using this for all my scripts fetching data.

---
layout: post
title: "djtemplate an opinionated django template"
tags: django djtemplate new-project
---

I've created a number of Django projects now and the most frustrating thing
jumping between projects is inconsistencies in the project layout.  So,
finally I set out to solve this problem.

It's called **djtemplate** and its located here:

    https://github.com/dlamotte/djtemplate

It's README contains instructions on how to use it, but basically, you do
something like this (make sure you're using Django 1.4+):

    django-admin.py startproject --template=https://github.com/dlamotte/djtemplate/tarball/master myprojectname

I call it a _very opinionated template_ because it's aimed at sites with
heavier javascript and chooses Backbone.js with Rivets.js among other
small libraries I've found useful.  The layout is also derived from
David Cramer's
[Settings in Django](http://justcramer.com/2011/01/13/settings-in-django/)
which I've found very useful.  The basic design is a typical
[Twitter Bootstrap](http://twitter.github.com/bootstrap/) site along
with [HTML5 Boilerplate](http://html5boilerplate.com/) baked in.

I didn't suspect it'd take me very long to setup this template honestly,
but after doing it and spending the better part of a day plus a few hours, I
noticed that even a basic setup like this actually takes quite a bit of time.
I'm hoping this template makes my projects much more consistent along with
saving me such time in setting up new projects.  I have to say, I find putting
the pieces together for a new project is very tedious.  Now, it'll be a
breeze.

Maybe others will find it useful (or at least as a base for your own personal
template).  Let me know if you do!

---
layout: post
title: "postgres performance"
tags: postgres performance tuning
---

Lately, I've been very interested in tuning and understanding database
performance.  Specifically, I use postgres and find that it has a number
of ways to track how it's performing and finding exactly what you're looking
for is complicated.

I follow **@craigkerstiens** on twitter who recently posted this excellent
[gist on github](https://gist.github.com/a0d9038fc5f86312ac9e).  It's a
few queries which help you better understand how well you're using your
indexes and caches.

I also use [pgbadger](http://dalibo.github.com/pgbadger/) to track many
things from the postgres logs.  Things like: what queries are the slowest,
what queries are taking up the most time, what queries are executed the
most, ... the list goes on and pgbadger is a great tool you should
definitely check out.  I'd used pgfouine in the past, but pgbadger
has won me over now as it's csvlog parsing actually works and it has
many more reports.

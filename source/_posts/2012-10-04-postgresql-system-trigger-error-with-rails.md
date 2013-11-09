---
title: PostgreSQL System Trigger Error with Rails
author: louis
layout: post
permalink: /2012/10/04/postgresql-system-trigger-error-with-rails/
categories:
  - Web Development
tags:
  - postgres
  - rails
---
`PG::Error: ERROR:  permission denied: "RI_ConstraintTrigger_50931" is a system trigger`

Just ran into this issue and had a hard time locating a solution, so just throwing it up here so others might be able to find it faster.

You might see this error message if:

*   You&#8217;re using PostgreSQL with Rails
*   You&#8217;re using foreign key constraints
*   You&#8217;re using <a href="https://github.com/bmabey/database_cleaner" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://github.com']);">database_cleaner</a> to tear down your test databases

The problem is this: Postgres sets up foreign key constraints as &#8220;system triggers,&#8221; which can only be removed by a superuser. When database_cleaner tries to empty out your database between test runs, it tries to remove these keys, which causes the failure.

Currently there isn&#8217;t a &#8220;real&#8221; fix for this, but Sergey Potapov has bundled together the required hacks into a gem called <a href="https://github.com/greyblake/rails3_pg_deferred_constraints" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://github.com']);">rails3_pg_deferred_constraints</a>. Pop it into your test group and you should be good to go.
---
layout: single
title:  "Local WP MySQL connection fix (after update)"
tagline: ""
date:   2025-11-17 14:48:48 +0100
categories: wordpress
tags: wordpress mysql local flywheel wp
excerpt: "Fix for when your Local sites throw a MySQL error after updating"
classes: wide
header:
  overlay_image: /assets/images/error-mysql-local.jpg
  teaser: /assets/images/error-mysql-local-teaser.jpg

---

{% capture notice-2 %}
tldr: [jump to the solution](#the-solution)
{% endcapture %}
<div class="notice">{{ notice-2 | markdownify }}</div>

I've been using Local (formerly, and more recognisably Local by Flywheel) as a local environment for developing
my WordPress plugins for years now. It's a great tool that allows you to spin up new WordPress sites very fast, and comes
with helpful tools out of the box such as Xdebug, a database manager, and a tool to catch and inspect outbound email.

I was still using the `6.7.2` version which had been working just fine, and held off on updating because that would usually
cause issues with my Xdebug config. 

But it had been bugging me to update to the latest version for a few years now, and recently having (and failing...) to use
the live link feature prompted me to finally install the latest version (3 major versions newer, version `9.2.9`).

The upgrade broke all the websites, and they displayed a MySQL connection error when trying to start them:

![mysql error message](/assets/images/local-mysql-error.png)

```
Error

Uh-oh! Unable to start site.

Error: Command failed:
C:\Users\mobil\AppData\Roaming\Local\lightning-services\mysql-8.0.16...\mysqladmin.exe
--host=:1 ping
mysqladmin: connect to server at '::1' failed
error: 'Can't connect to MySQL server on '::1' (10061)'
Check that mysqld is running on ::1 and that the port is 10005.
You can check this by doing 'telnet ::1 10005'

    at genericNodeError (node:internal/errors:983:15)
    at wrappedFn (node:internal/errors:537:14)
    at ChildProcess.exithandler (node:child_process:417:12)
    at ChildProcess.emit (node:events:519:28)
    at maybeClose (node:internal/child_process:1101:16)
    at ChildProcess._handle.onexit (node:internal/child_process:304:5)

```
Newly added sites were working just fine, and I noticed they were using MySQL version `8.0.35` instead of the 
`8.0.16` that my existing sites were using. 

However, the site config files made no reference to the MySQL version, and there was no way to change the MySQL version in
the Local UI. 

I did some Googling and the only "solution" I could find was to ZIP every site folder and re-import them. This ...worked,
but surely there must be an easier and less dumb way, right?

### The Solution

I dove into the Local config files in `AppData\Roaming\Local` and found the MySQL version for each site in the `sites.json`
file. All I did was change it to `8.0.35` for just _one_ of the sites, restarted Local, and this actually made _all_ sites work again.
But if you want to be thorough, it might be a better idea to update the MySQL version everywhere.
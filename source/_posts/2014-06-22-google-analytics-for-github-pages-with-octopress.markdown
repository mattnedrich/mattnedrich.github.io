---
layout: post
title: "Google Analytics for Github pages with Octopress"
date: 2014-06-22 16:56:10 -0400
comments: true
categories: Octopress
---

I wanted to setup google analytics for my octopress blog hosted on [github pages](https://pages.github.com/). My situation was a little different than some of the existing tutorials because I'm using a custom domain, not the standard <code>username.github.io</code> domain. To get everything setup and working I did the following:

1. Create a [Google Analytics](http://www.google.com/analytics/) account.
2. Tell your analytics account to monitor your custom domain. For me this was my <code>www.mattnedrich.com</code> domain, not the <code>mattnedrich.github.io</code> one.
3. Update your Octopress <code>_config.yml</code> file to include your analytics tracking id by setting the <code>google_analytics_tracking_id:</code> property.
4. Generate and deploy your site

There are several [posts](http://rebootjeff.github.io/blog/2013/09/17/adding-google-analytics-to-octopress-blog-on-github-pages/) online that talk about modifying the <code>google_analytics.html</code> Octopress file. If you are using a custom domain with your github page you don't have to do this (or at least I didn't have to).

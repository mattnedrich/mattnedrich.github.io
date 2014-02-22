---
layout: post
title: "Finding Files on Windows"
date: 2014-01-18 16:53:50 -0500
comments: true
categories: [Windows, Grep] 
---

I often find myself in situations where I know a file exists but can't find it.

On Windows, the following can be used to search for a file from the current working directory

{% coderay %}
dir /s /b | findstr [search string]
{% endcoderay %}

Here <code>/s</code> denotes a recursive search, and <code>/b</code> allows the full paths of the files to be printed (omit that if the full paths are not desired). The results from the <code>dir</code> are piped to a <code>findstr</code> command where a filter can be applied to find the desired file(s).

If you have <code>grep</code> installed (e.g., via the [unix utilities](http://sourceforge.net/projects/unxutils/)) you can replace <code>findstr</code> with <code>grep</code>

{% coderay %}
dir /s /b | grep [search string]
{% endcoderay %}

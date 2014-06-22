---
layout: post
title: "Managing Octopress from different local repositories"
date: 2014-06-22 13:04:11 -0400
comments: true 
categories: Octopress
---

I wanted to manage my octopress blog from different computers. I have my blog source on github, so I thought it would be as easy as pulling it, making changes, generating, and deploying. However, I ran into several nuanced issues along the way which resulted in this error when I tried to deploy:

{% coderay lang:bash%}
## Pushing generated _deploy website
To http://github.com/mattnedrich/mattnedrich.github.io
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'http://github.com/mattnedrich/mattnedrich.github.io'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
{% endcoderay %}

After being confused for a while, I came across this [post](http://blog.zerosharp.com/clone-your-octopress-to-blog-from-two-places/) and was able to successfully pull, modify, and deploy my blog from a second local repository. The only change I had to make from the instructions in the post was that I had to clone the master branch into the _deploy directory <b>after</b> running <code>rake setup_github_pages</code> as pointed out in the comments.


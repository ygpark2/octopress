---
layout: post
title: "when rake new_post command does not work in zsh"
date: 2012-10-22 10:15
comments: true
categories: zsh, octopress
---

To work around the problem, use below command

{% codeblock lang:bash %}
rake new_post\["My new post"\]
{% endcodeblock %}

Another way to avoid the problem is to add next configuration in your .zshrc

{% codeblock lang:bash %}
alias rake='noglob rake'
{% endcodeblock %}


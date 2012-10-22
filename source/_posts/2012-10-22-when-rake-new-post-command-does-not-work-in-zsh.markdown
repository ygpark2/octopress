---
layout: post
title: "when rake new_post command does not work in zsh"
date: 2012-10-22 10:15
comments: true
categories: 
---

To work around the problem, use below command

{% codeblock lang:shell %}
rake new_post\["My new post"\]
{% endcodeblock %}

Another way to avoid the problem is to add next configuration in your .zshrc

{% codeblock lang:shell %}
alias rake='noglob rake'
{% endcodeblock %}

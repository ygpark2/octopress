---
layout: post
title: "case statement in ruby"
date: 2012-10-23 10:40
comments: true
categories: ruby, case
---

Ruby case ... when statement use the === operator internally.

{% codeblock lang:ruby %}
obj = 'hello'
case obj.class
when String
  print('It is a string')
when Fixnum
  print('It is a number')
else
  print('It is not a string')
end
{% endcodeblock %}

{% codeblock lang:ruby %}
obj = 'hello'
case obj  # was case obj.class
when String
  print('It is a string')
when Fixnum
  print('It is a number')
else
  print('It is not a string')
end
{% endcodeblock %}


{% codeblock lang:ruby %}

{% endcodeblock %}

Without parameter
{% codeblock lang:ruby %}
case
when b < 3
  puts "Little than 3"
when b == 3
  puts "Equal to 3"
when b === (1..10)
  puts "Something in closed range of [1..10]"
end
{% endcodeblock %}

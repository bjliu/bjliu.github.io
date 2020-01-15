---
layout: post
title: Learning Ruby and Rails
published: true
---
I recently started a new job at [Printavo](https://www.printavo.com/) and they even made a nice [blog post](https://www.printavo.com/blog/welcome-ben-liu-to-printavo) about me. So far so good! It's by far the smallest team I've ever worked from (13 people) and I've worked at VMware, Workday, and Expedia, which are all ginormous.


One of the biggest attractions coming in to Printavo was learning about Ruby on Rails. Rails was a huge draw for me because its core idea of ["convention over configuration"](https://rubyonrails.org/doctrine/) means that I can focus on building a web app quickly. And Rails has a huge presence in smaller, remote startup type companies like Printavo.


Of course, there's all the naysayers who say: "Isn't Rails dead?" or "I heard that Rails doesn't scale well." or "Ruby is too much magic and too much chaos."
What I've learned though is that scability is about scaling horizontally and that performance boosts associated with "faster" languages (one type of vertical scaling) might give a 10-20% improvement, but isn't the core issue for the most part. In fact, Rails is still being used by many prominent web applications, such as Shopify, Airbnb, and Github.


So I'm excited to get started and to learn!
I picked up several books, including Programming Ruby (the pickaxe book) by Dave Thomas and The Rails Way by Obie Fernandez to name a few. One of the main advantages to Ruby is its elegant syntax made possible by the idea of *code blocks*, which are chunks of code that can be treated as parameters.


Many of the looping constructs built into Java and C become method calls in Ruby because of this. That means Ruby doesn't really need for loops most of the time, which makes it a truly object oriented language. This was mind-blowingly interesting stuff to me.
```ruby
['cat', 'dog', 'horse'].each {|name| print name, " " }
5.times { print "*" }
3.upto(6) {|i| print i }
{'a'..'e'}.each {|char| print char }
puts
```


Ruby also has duck typing (common to dynamic programming languages like Python) which means that you don't need to worry about bounding your variables to a certain type.


As for Rails, I've learned about how tied Rails is to REST and how the routes that you set are for 2 purposes: link creation and mapping HTTP requests to controller actions.


I'm excited to get into the meat of things and to learn about controllers, ActiveRecord, and ActionView in Rails, while deepening my knowledge of Ruby.
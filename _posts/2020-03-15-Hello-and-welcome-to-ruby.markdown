---
layout: post
comments: true
title:  "Hello and welcome to Ruby!"
date:   2020-03-15 13:25:51 -0700
categories: ruby methods software
---
As I recuperate from week 1 at the Flatiron School and prepare for my first __Code Challenge__, I decided to take a break from the labs and reflect on what I've learned, and how I've learned it. I came to Flatiron having taken several college courses in programming and software engineering. Those courses were administered in a very similar way:

1. Start in a robust, object-oriented language. In my case Java and then C++.

2. Strip the language down to its bare minimum, removing all standard libraries such as Java's JCL and the stl for C/C++.

3. Teach core data structures and algorithms by making the students build them from scratch. If I needed a linked list, I had to build one. Need a sorting algorithm? Better be ready to build that too.

4. Roll these concepts into larger projects that must be completed on tight timelines.

5. Make plagarism both a nebulous and a high-consequence issue. Most of my classes were so concerned with potential plagarism that study groups and collaboration virtually ceased to exist, leaving students to struggle on their own or rely exclusively on their instructors for guidance.

While my inherent bias is probably apparent by now, suffice to say that I struggled with this kind of format, and writing code became a time consuming, demoralizing, and generally stressful affair. Ok, rant over, I'm sure there are plenty of successful programmers who learned this way, I just didn't happen to be one of them. I had to find a different way: 

Enter [Ruby!](https://www.ruby-lang.org/en/ "A Programmer's Best Friend")

Flatiron takes quite a different approach to teaching students how to code, and part of that new approach is using the many cool features that Ruby has to offer. A couple of my personal favorites are *block syntax* and *method chaining*. 

Here is a quick comparison of two ways I wrote the same method:

{% highlight ruby %}
def backers
  result = []
  ProjectBacker.all.select do |instance|
    if instance.project == self
      result << instance.backer
    end
  end
end
#=> An array of people backing a particular project
{% endhighlight %}

I'm stuck between two worlds. I know the power of a chained method call like `ProjectBacker.all.select` to allow me to iterate over a collection, but why am I creating an array and then duplicating all the instances of objects meeting my criteria in order to return a result? I knew that I could improve.

{% highlight ruby %}
def backers
  ProjectBacker.all.map {|instance| instance.project == self ? instance.backer : nil}.compact
end
#=> An array of people backing a particular project
{% endhighlight %}

I'm definitely still new, so I hope more talented Rubyists aren't cringing right now, but man, doesn't that seem like an improvement? Instead of using `select` and then having to mutate the results with additional logic, I can use a `map` to do that for me. I can also nest my block syntax in a one liner and combine it with a ternary operator (`?:`) to do my comparison. No unnecessary object duplication now. My only regret is the `nil` returned by the ternary if a given instance doesn't meet my condition. It clutters my array and requires an additional `compact` method call to get me the result I wanted. If anyone knows how to write a slick single condition one liner, I want to see it!

I wish I could say this next example is my own, but it's not. What it is, however, is a great example of Ruby's powerful economy of expression.

{% highlight ruby %}
def best_tipper
    best_tipped_meal = meals.max do |meal_a, meal_b|
        meal_a.tip <=> meal_b.tip
    end
    best_tipped_meal.customer
  end
  #=> the customer who gave this waiter the highest tip
{% endhighlight %}

I really like how this method combines iteration, block syntax, and three way comparison to yield metrics about object relationships. Instead of having to track stats like best tipper, number of customers, and the like, Ruby provides robust syntactic sugar to generate these results on the fly. Pretty cool stuff!

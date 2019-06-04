---
layout: post
title: "Get to Know Your Self"
date: 2018-08-26 13:23:53 -0400
permalink: get-to-know-your-self
featured-img: "/static/img/post-images/simple-self.png"
categories: [Flatiron School]
tags: [software engineering, Ruby, keywords]
excerpt: <p>While Ruby is an incredibly user-friendly language, it’s not without its conundrums&mdash;one in particular being <code>self</code>. <code>self</code> is a Ruby keyword that can be scoped to any instance or class. This enables developers to contextually reference a particular instance or class&mdash;depending on what the needs are for their program&mdash;without using a specific variable name. It's not hard to see how this could quickly get confusing. Read on to walk through some simple examples and gain a better understanding of the concept of <code>self</code>.</p>
---

While Ruby is an incredibly user-friendly language, it’s not without its conundrums—one in particular: `self`. `self` is a Ruby keyword, or a word specifically designed to have a special meaning in the Ruby language. The scope (or program visibility) of `self` is any class or instance of a class (also called objects). This means that `self` can enable developers to contextually reference a particular class or instance of a class—depending on what the needs are for their program—without using a specific variable name.

Here’s a very simple example of `self` referring to a class and `self` referring to an instance of a class:

![Simple example of using `self`](/static/img/post-images/simple-self.png)

If the method is a message, who is the receiver? The object, or `self`! In this example, `self` is receiving instruction from other parts of the code and applying it to the current object being used in the program. Here’s how:

**`self` referring to an instance of a class**

We first see `self` on line 11 in the `#initialize` method. Here, we are shoveling `self` into the `@@all` class variable. What does this mean? In this part of the program, `self` is referring to the new instance of class that is being created with the `#initialize` method (in this case a new species). As you can see in lines 21&ndash;23, we create three new instances of the Animals class. As each instance is created, they are automatically saved into the `@@all` class variable. In other words, instead of having to write: `@@all << tiger`, `@@all << monkey`, `@@all << giraffe` for each instance we want to add to the `@@all` class variable, we can write `@@all << self` which will execute this same line of code for any instance of the Animals class.

**`self` referring to a class**

We next see `self` on line 14 in the `#self.all` class method. This is a class method designed to return the value of the `@@all` class variable, which is an array of instances of the Animals class. As you saw above, we created three new instances of the Animals class: tiger, monkey, and giraffe (which are displayed on lines 1&ndash;3 of the `pry`). On line 4 of the `pry`, we call the `#self.all` class method on the Animals class by entering `Animals.all`. The program is recognizing that in this case, `self` is referring to the Animals class and returning the expected array of the tiger, monkey, and giraffe instances.

So how does `self` know when to change the object it’s referencing? Two ways:
1. The first being context as we have previous discussed above.
2. The second being a built-in rule for this keyword: ***one and only one `self` will exist at any given time in the program***. There will never be a case in which `self` will refer to more than one object at a time, therefore leaving it up to context to determine what `self` actually means at each point in the program.

I know, that’s a lot of information and your head is probably spinning by now, but let’s do one more example of `self`. I’ll walk you through it!

![More complex example of using `self`](/static/img/post-images/ice-cream-self.png)

In defining the Creameries class, we create attribute readers for flavors, name, and flavor. Flavors is an instance variable that’s initialized as an empty array, meant to hold all the ice cream flavors of a particular creamery. We don’t want to accidentally overwrite the `@flavors` array and change its data type, so it is set as a reader. Once the creamery is created, its name won’t be changed (likewise with each flavor name) which is why they’re set as readers. We then create an `@@all` class variable to store new creamery objects (which are instances of the Creameries class) for future use.

The `#initialize` method creates a new creamery given the creamery name and an ice cream flavor. Here we set our instance method for `#name`, which returns the name of the creamery it calls upon. Then we declare an empty array called `@flavors` to store this creamery’s ice cream flavors. The ice cream flavor is added to the creamery’s `@flavors` instance variable and the creamery itself is added to the `@@all` class variable.

The `#new_flavor` method creates new flavors for each creamery. The new flavor is then added to the creamery’s `@flavors` instance variable.

Lastly, the `#self.all_flavors` class method iterates over the creameries in the `@@all` class variable and displays all the flavors in the Creameries class. It works like this: We access the `@flavors` instance variable for each creamery in the Creameries class (stored in the `@@all` class variable), which gives us an array of flavors for each creamery. As a result, we `puts` each flavor from each `@flavors` array and return the original `@@all` class variable.

After defining the Creameries class, we create three new creameries: bnj, hersheys, and nelsons.

In the above example, we see `self` in lines 12, 13, 18, and 22. In which lines is `self` referring to an instance of the class? Lines 12 , 13, and 18. In lines 12 and 18, we’re adding an ice cream flavor to the `@flavors` instance variable of a creamery, which you can see in lines 1&ndash;3 of the `pry`. In line 13, we’re adding the creamery itself to the `@@all` class variable (since it’s an instance of the Creameries class). In line 22, `self` is referring to the Creameries class by calling the `#self.all_flavors` class method. This method uses the `@@all` class variable to `puts` all of the ice cream flavors in the Creameries class and return an array of Creameries instances.

You just learned `self`! If you feel you have a good grasp of the concept of `self`, celebrate with a scoop of your favorite ice cream! If not, check out the following helpful resource: [Self in Ruby: A Comprehensive Overview](https://airbrake.io/blog/ruby/self-ruby-overview)

...And then have a scoop of ice cream. :D
---
layout: post
title:      "To @ or to @@"
date:       2018-08-18 20:03:21 +0000
permalink:  to_at_or_to_at_at
---


Some days are better than others. I'll be flying through coursework and riding high, and then one little lab can send me crashing 30,000 feet.  Sometimes you gotta step back, take a deep breath, and take another look at what you're really doing with all these symbols and methods.  For example, @ versus @@ -- what do they mean again?

According to a [stackoverflow.com article](https://stackoverflow.com/questions/5890118/what-does-variable-mean-in-ruby/5890199#5890199) on the subject, the @@ class variable gets applied to all instances of a class.  If you change the value of the class variable, that value becomes the new master.  Now, let me illustrate:


    class Mean_girls

    @@dress_code = "pink"

    def dress_code=(dress_code)
    @@dress_code = dress_code
    end

    def dress_code
    @@dress_code
    end

    end

Above is the method.  Below is what I wish to call from the method:

      m = Mean_girls.new
      puts "On Mondays, we all wear #{m.dress_code}." 
		
      t = Mean_girls.new
      t.dress_code = "glitter scrunchies"
      puts "On Tuesdays, we all wear #{t.dress_code}."
		
     w = Mean_girls.new
     w.dress_code = puts "On Wednesdays, we wear even more #{m.dress_code} and #{t.dress_code}."
		
    th = Mean_girls.new
    th.dress_code = "six-inch heels"
    puts "On Thursdays, we MUST wear #{m.dress_code}."
		
    f = Mean_girls.new
    f.dress_code = "hoop earrings"
    puts "On Fridays, we wear #{f.dress_code} or FEAR THE CONSEQUENCES!"

And here are the results:

    On Mondays, we all wear pink.
    On Tuesdays, we all wear glitter scrunchies.
    On Wednesdays, we wear even more glitter scrunchies and glitter scrunchies.
    On Thursdays, we MUST wear six-inch heels.
    On Fridays, we wear hoop earrings or FEAR THE CONSEQUENCES!
    => nil


As you can see in the "On Wednesdays..." line, it doesn't matter if I interpolate "m.dress_code", "t.dress_code", or "w.dress_code".  It's all "glitter scrunchies".  Mean girls love their glitter scrunchies.  At least in the world of figure skating or '80s youth dance ensembles, maybe.  Again, with "On Thursdays...", it doesn't matter which previously established "dress_code" variable you interpolate because the class variable is inflexible.

Now, look what happens if I switch out @@ for the @ instance variable with the same example:


    class Mean_girls
  
    @dress_code = "pink"

    def dress_code=(dress_code)
    @dress_code = dress_code
    end
  
    def dress_code
    @dress_code
    end

    end


In this case, I must define m.dress_code, or else it will come up blank when I interpolate.  So, let's define m.dress_code = "red".  Also, let's define w.dress_code = "black eye liner" and rewrite, "On Wednesdays, we wear even more #{m.dress_code}, #{t.dress_code}, and also #{w.dress_code}, for a look that kills."

So:

    m = Mean_girls.new
    m.dress_code = "red"
    puts "On Mondays, we all wear #{m.dress_code}." 
		
    t = Mean_girls.new
    t.dress_code = "glitter scrunchies"
    puts "On Tuesdays, we all wear #{t.dress_code}."
		
    w = Mean_girls.new
    w.dress_code = "black eye liner"
    puts "On Wednesdays, we wear even more #{m.dress_code}, #{t.dress_code}, and also #{w.dress_code}, for a look that  kills."
		
    th = Mean_girls.new
    th.dress_code = "six-inch heels"
    puts "On Thursdays, we MUST wear #{m.dress_code}."
		
    f = Mean_girls.new
    f.dress_code = "hoop earrings"
    puts "On Fridays, we wear #{f.dress_code} or FEAR THE CONSEQUENCES!"

Here are the results:


    On Mondays, we all wear red.
    On Tuesdays, we all wear glitter scrunchies.
    On Wednesdays, we wear even more red, glitter scrunchies, and also black eye liner, for a look that kills.
    On Thursdays, we MUST wear red.
    On Fridays, we wear hoop earrings or FEAR THE CONSEQUENCES!
    => nil

As you can see, instance variables ("@") are more dynamic.  And red is most fetch.

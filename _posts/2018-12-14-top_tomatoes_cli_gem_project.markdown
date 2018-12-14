---
layout: post
title:      "Top Tomatoes CLI Gem Project"
date:       2018-12-14 15:26:45 +0000
permalink:  top_tomatoes_cli_gem_project
---


I moved to Minneapolis from New York City six months ago and the number one thing I miss is the Nitehawk Cinema in Brooklyn. In other words, I love the movie-going experience, and the Nitehawk is the ultimate. So, picking the Rotten Tomatoes film review site was an obvious choice for scraping information for my CLI Gem project.  

I decided to focus on the top box office list, since I find it interesting that a film like Fantastic Beasts: The Crimes of Grindelwald, which has a very low 38% approval rating, is nevertheless #4 on the top ten list. Goes to show the near impenetrability of the Harry Potter fan base, which is a hot button topic I will not touch further here. 

To get started, I first watched this video tutorial on [setting up an in-browser IDE](https://www.youtube.com/watch?time_continue=317&v=YZNXWWHUO-E).  Then I watched Avi Flombaum's [CLI Gem Walkthrough](https://www.youtube.com/watch?v=_lDExWIhYKI) to help get the code flowing.  I also consulted other videos and browsed past students' projects on Github to expand my breadth of knowledge, understanding, and ideas. In the end, that may have hurt me because too many methods and ideas piled up in my head and obstructed mental clarity. I believe I knew what I was doing all along, but lacked the confidence to sit alone with my code and BE PATIENT. 

In addition to Avi's CLI Gem Walkthrough video, which IS a useful jumping off point, I found the most help came from live conversations with others, whether it be a fellow student I met on Slack or the section lead.  I spent two and a half hours on Zoom with a student one night simply trying to figure out how to correctly scrape film titles into an array of individual instances, as opposed to one long string. Ultimately, the working code we built together had to be amended. Regardless, I'm astounded by and eternally grateful for this stranger's dedication to my little project. To write ONE line of code.  

Anyway, after some tears and screaming into a pillow, I finally produced a functional and smooth program. And it feels good. And oddly simple, looking at it now. Access to the full project code is here in [this Github link](https://github.com/caroberg/top_tomatoes). The following is a breakdown of my classes and their methods... 

Scraper Class:
* The self.scrape_top_films class employs the ".collect" iterator, versus the ".each" iterator, to organize an array of individual film instances from Rotten Tomatoes' list of top box office films.
* The first method utilizes a "while" loop with input "i" to scrape the correct values of "title", "review_rating", "box_office_revenue", and "film_url" corresponding to its film instance. 
* The first method creates a relationship with TopTomatoes::Film.new(title, review_rating, box_office_revenue, film_url). 
* The self.scrape_film(film) method takes in the now existing film instances as arguments and scrapes data from their corresponding film pages to provide more details on the individual films.

Film Class: 
* Very simple setup for this class.  Essentially, all I did was create a list of attr_accessor elements, defined them, and created the array "@@all".

CLI Class: 
* The "call" method welcomes the user, calls TopTomatoes::Scraper.scrape_top_films, as well as a "list_films" method and "menu" method to follow.
* Initially, I had TopTomatoes::Scraper.scrape_top_films kick off the "list_films" method definition. However, I discovered that when I called "list_films" for a second, third, etc., time, I would get a list of 1-20, instead of 1-10, with the film list repeating itself twice. So, the "call" method is where that one belongs. Meanwhile, I connected the ".each.with_index(1)" iterator to all the instances of the Film class to create a numbered list of films and their basic attributes. 
* In the "menu" method definition, I employed "while" and "if" loops to create multiple opportunities for the user to access more information on individual films. To list the individual film, I paired the variable "film" with TopTomatoes::Film.all[input.to_i-1] to connect with TopTomatoes::Scraper.scrape_film(film). Of course, it's important to remember that the user's input might be "5", but the computer must be manipulated to read "4" to get the correct corresponding instance in the array. Hence, the user's input is converted into an integer and subtracted by one.

To see the program in action, here is a [video walkthrough](https://www.youtube.com/watch?v=3cb4VYA5ed8) of Top Tomatoes.

Going in and coming out, this project is deceptively simple. I cannot express enough how important live human interaction was for me in completing this project. With the holidays coming up, I will be giving thanks for all I learned and the people who helped me through the struggles.

Onward and upward!

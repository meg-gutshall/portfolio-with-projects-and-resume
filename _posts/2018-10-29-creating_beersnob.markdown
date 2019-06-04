---
layout: post
title: "Creating BeerSnob"
date: 2018-10-29 13:49:46 -0400
permalink: creating-beersnob
categories: [Flatiron School, portfolio projects]
tags: [software engineering, CLI, command line interface, projects]
excerpt: <p>Read through the process of how I created my first project at Flatiron where app users can choose from a predefined list to learn about different beer styles and their characteristics. I detail the steps I took to set up the project, the function of each program file, and some struggles I ran into along the way.</p>
---

## Project Idea

I had a few ideas that I was considering for this project at first. I tossed around the idea of scraping through each year in the '90s and highlighting different trends, fads, pop culture, and new stories. I also considered scraping through popular songs and returning the original songs that the new song took samples from. That came closest to fruition but unfortunately I couldn't find a website conducive to this idea. I added it to my list of future projects. :)

I wanted my first project to be something useful&mdash;not just a throw-away program that I’d never touch after submission. I examined my interests and found that I could create an app at the intersection of beer and education. I found craftbeer.com to be a viable website for scraping and thus BeerSnob was born.

## Starting the Project

I experienced quite a few delays in starting this project, each one pretty frustrating. I used the Visual Studio Code text editor in place of the Learn IDE and had several issues with setup that I had to solve–either through Google, message boards, or speaking with other developers and students. After fixing the settings to allow me to submit commit messages with titles and description (which is a coding habit I want to ingrain upon myself from the start) and then installing rvm to manage my rubies and gems, I was ready to dive in and begin coding my project.

### Setup: Creating a Gem with Bundler

I installed and used bundler to set up my gem, taking the following steps

1. Open the terminal and `cd` into the folder in which you want to create your gem (mine is called `code`).
2. Type in `bundle gem` then your gem name. (Use an underscore instead of a hyphen to make your gem name camel case.) Decide whether you want to use `rspec`, `minitest`, or neither for development testing, use the MIT license for open sourcing regulation, and a code of conduct for any open source contributors to follow. Bundler then initializes a git repo for your gem!

### Setup: Connecting Your Gem to GitHub

1. `cd` into your newly created gem folder and navigate to GitHub. Create a new repository with the same name as your gem. Type `git add .` into your terminal, then enter the last three lines in the "create a new repository on the command line" option displayed on the new page loaded in GitHub for your gem repository.
2. Refresh your GitHub page and you'll see your gem and its files now in your GitHub repository.
3. To make sure you're working out of the correct repo, click on the green "Clone or download" button in your gem's repository. Make sure that "Clone with SSH" is selected and not "Clone with HTTPS". Click on the clipboard image to copy the link and return to your terminal. Make sure you're working in the `code` folder (or whichever folder your gem is stored in), then type `git clone` and paste the link to clone the correct repo for your gem project.
4. Your new gem's repo folder should appear in your local `code` folder. Open your text editor of choice and `cd` into the gem folder to start working.
_Hint:_ To test your text editor's connectivity to GitHub, make a change to your `README.md` file and save. In the terminal, type `git add .`, `git commit -m "Modify README.md"`, and `git push`. When you refresh your GitHub page, you should see the updated commit message and two commits.

### Setup: The Executable File

1. Add your executable file (the file that makes the app run) in the `bin` folder. I named mine `beer-snob`. (Remember to add the shebang line `#!/usr/bin/env ruby` at the top of your file so your computer knows to use ruby to run the program.)
2. We want the user to be able to run the app right out of the terminal using bash instead of explicitly calling on a ruby interpreter to run it. This means we have to change the executable file's permissions. We can do this by `cd`ing into the `bin` folder and entering `chmod +x` followed by the executable file's name.

## Program Files

I employed the encapsulation principle in creating and writing my various program files. All components of the app (gems and files) are required in an environment file called `beer_snob.rb`, which in turn is required in the executable file.

### BeerSnob Executable File

The app's executable file employs only one line of code to start the program, however, it does have a few other requirements:
1. As mentioned earlier, the shebang line `#!/usr/bin/env ruby` tells your computer that the file is written in ruby. Therefore, this needs to be the very first line of your program to ensure that your computer reads and processes the app correctly.
2. The next line is `require bundler/setup` which enables you to load your gems by setting their load paths.
3. The third line is your environment file. Mine reads `require beer_snob`. This enables your executable file to access all the gems and program files contained in your environment.
4. Then of course follows the line that starts the program. Mine reads `CLI.new.learn`.

### CLI File

The purpose of the CLI file is to call methods that act as basic functions of the program. I created a basic run method `#learn` which first instantiates an instance variable using the `Beers` and `Scraper` classes and then uses that variable in the following methods:
- `#greeting`: welcomes the user to the app
- `#list_style_families`: lists the 15 beer style families
- `#top_menu`: handles the user input of which family they wish to explore and executes the appropriate action
- `#list_beer_styles`: lists the beer styles within whichever style family the user chose
- `#sub_menu`: handles the user input of which beer style they wish to explore and returns the appropriate information
- `#exit`: displays a closing message and exits out of the program

I incorporated decorative elements in my `puts` Strings by using ASCII art in the `#exit` method and the `colorize` gem to distinguish headers and categories from general information.

### Scraper

I decided to start my project by creating a `scraper.rb` file. I actually began the file in a practice repo to mess around and see what I could come up with. When it started coming together, I migrated my work into a file in my gem repo.

For each beer I wanted to store the style name, family name, and a brief description of its characteristics. Since some beers have a longer description than others, I at first decided to store only the first paragraph of each beer description. Through helping another Flatiron student with her project, I discovered a method which could return multiple paragraphs of text, depending on the parameters I set. I liked this execution much better as it allowed the app to pull all text for the beer styles with a multi-paragraph description without compromising other, shorter beer style description texts.

Next I wanted to store the listed examples of brewery and beer name for each style. This was a little tricky because my scraper kept returning the list of breweries followed by the list of beer names instead of returning them as key/value pairs. In the end, I created a nested array composed of brewery/beer name hashes by creating an iteration inside of the main beer style iteration. By doing this, I was able to create and store key/value pairs of the commercial examples.

### Beers File

I wrote the `beers.rb` file as abstractly as possible to enable it to create new arrays and object attributes from whichever hash arguments were fed into the method. These methods in turn created the objects that were used in the `cli.rb` file to display the scraped information.

### Art File

The `art.rb` file houses ASCII art which I incorporated in my `CLI#exit` method.

## Troubleshooting

### Scraping

For you to be able to get a sense of the document structure of the website I was scraping from, I'm including the site link here: [Beer Styles Study Guide](https://www.craftbeer.com/beer/beer-styles-guide)

The site that I decided to scrape is structured differently than I would have done it. Not all the HTML elements are nested properly and a lot of them either don't have classes or they're not descriptive of the actual element. Therefore, when scraping the site, I originally decided to iterate over the `<li>` tags and use indexes to draw out the information and associate it to the proper key. Unfortunately, some of the beer styles include the "Brewing and Conditioning Process" characteristic while other do not. I discovered this after my CLI displayed info associated with the wrong characteristic for some of the beer styles, shifting them down by one line item. So I went back to the scraper drawing board.

I reviewed earlier scraper labs and got an idea from the Student Scraper Lab. I created a subiteration within my `#scrape` method to create a key/value pair only if the key existed. I assigned the new key to be created from the `param` class and its corresponding value from the `value` class nested within the `<li>` tags. This process scraped more information than I want to display, but that wasn’t an issue since I could choose which information I wanted to display in the `CLI` class.

I didn't have to change anything for the “Commercial Examples” characteristic since I also created a subiteration for those, which dug deeper down in the nested levels to retrieve the specific beer and brewery information. However, I did have to treat the “Alcohol” characteristic differently because instead of storing its value in a `<span class="value">` element, the website inserted the value text directly inside the `<li>` element. Thankfully, I was able to keep my indexing method for this characteristic as it's the first line under the "Style A-Z" category and, therefore, is unaffected by the addition of other characteristics.

### Simplifying the Program

Even after making the above changes I still ran into issues. When writing the `#initialize` method in the `Beers` class, I employed the `#send` method in order to create the object’s attributes from the key/value pairs. However, since the keys were Strings and not Symbols, I wasn’t able to convert the pairs to attributes using the `#send` method. I could probably figure out a way to do it if I researched it more but I decided to eliminate these characteristics altogether and readdress the problem at a later date.

## Project Demo Video

<iframe width="100%" frameborder="0" src="https://www.youtube.com/embed/m-sAVQi9MQo" allowfullscreen></iframe>
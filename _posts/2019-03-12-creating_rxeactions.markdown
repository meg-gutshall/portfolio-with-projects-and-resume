---
layout: post
title: "Creating Rxeactions"
date: 2019-03-12 16:58:52 -0400
permalink: creating-rxeactions
categories: [Flatiron School, portfolio projects]
tags: [software engineering, Sinatra, projects]
excerpt: <p>Read through the process of how I created my second project at Flatiron where app users can privately track medications they’ve been prescribed as well as side effects, thoughts, and feelings associated with those medications. I share where the idea for this project came from and how I got started with setting up the project. Then, I get into the build, breaking it down by the MVC (Model View Controller) file structure. To end the post, I touch on styling a little bit, I share some troubleshooting issues that I experienced and I wrap up with expansion features I hope to add on in the future.</p>
---

## Project Idea

When I was in college, I was diagnosed with generalized anxiety disorder. Since then, I have changed psychiatrists several times and have added major depression and PTSD to my list of diagnoses. Over the course of the past decade, between seeking treatment from different doctors (each with their own preferred methods of therapy) and receiving new diagnoses, I've been on a LOT of different medications. Unfortunately, I couldn't tell you what most of them were, let alone the dosage.

This is where the idea for Rxeactions stemmed from. When I would see a new psychiatrist and we would discuss prescription medications, they would ask me for a history which I wouldn't be able to give them. I may be able to track down at least the names, dosages, and when I was taking them, but who wants to call a bunch of doctor's offices to track down their medical records? I don't! Other times, my psychiatrist would suggest a medication they had in mind to try for my treatment and I'd remember taking it before but not when, or for how long, or why I stopped it.

I really wish I would have had the foresight at the time to write down my prescribed medications and associated experiences, but I didn't. With this app, it's no longer something I'll have to worry about and hopefully it will prevent the same regret I feel now for others who find themselves in a similar situation to mine.

## Starting the Project

### Delay

I had a really difficult time starting this project. I think it was a combination of a few things. The last three labs in Flatiron's Sinatra section of the curriculum&mdash;[Playlister](https://github.com/meg-gutshall/playlister-sinatra-v-000), [NYC Landmarks](https://github.com/meg-gutshall/nyc-sinatra-v-000), and [Fwitter](https://github.com/meg-gutshall/sinatra-fwitter-group-project-v-000)&mdash;were just _ridiculously_ hard. I got totally burnt out from them so by the time I finished those I was mentally exhausted. Also after I finished Fwitter, I didn't get an email from the project lead for another four days! When I asked him about it, he told me about the process the project leads have to go through to see which students are at the project and it's just really jacked up. Flatiron School's logistics and operations department needs to get their shit together. They're churning out these excellent programmers–why are they not having them build out solutions for their own use as well? It just doesn't make any sense to me why they are using sub-par third-party tools when former students can make them exactly what they need. Okay, rant over.

### Design

I designed my app to act as a digital journal that allows users to privately track medications they’ve been prescribed as well as side effects, thoughts, and feelings associated with those medications.

## The Setup

The `environment.rb` file is where we load all of the app's dependencies, from gems to database connections. Some Ruby gems that I decided to use were `BCrypt` for password hashing and `Sinatra Flash` to display error messages. The `config.ru` file details to Rack the environment requirements of the app and then runs the app. The `Rakefile` is where the app's rake tasks are defined. The only custom task defined in my app's `Rakefile` is `rake console`, which starts a new session in `Pry`.

For more detailed information on the aforementioned files and how they interact, see my blog post [What's Up With That!?: SINATRA_ENV](http://feralcodephilly.com/whats_up_with_that_sinatra_env.html).

## The Build

### Corneal

I used the [`Corneal` gem](https://github.com/thebrianemory/corneal) to build out the skeleton of my project. It was so helpful, and no wonder&mdash;it was build by a previous Flatiron student!

## The Build: Models

### Migrations

I built out my migration tables using the `rake db:create_table` rake task. I love the autocreate feature for the migration tables because writing those timestamp names by hand is annoying!! I had to go through a lot of iterations of my `create_table` migrations since I kept changing up my model attributes.

### Models

I built out three models: `User`, `Medication`, and `Reaction`. I set up my associations as follows: A user has many medications and has many reactions through medications. A medication belongs to a user and has many reactions. A reaction belongs to a user. Therefore, I added a `user_id` attribute to the medication table migration and a `medication_id` attribute to the reaction table migration.

In addition, the `Medication` model contains two methods: one that defines the medication's `slug` attribute and one that will find a medication by its `slug`.

#### ActiveRecord Validations

I added validations to my models as well using ActiveRecord's built-in validation helper methods. Model-level validations are the best way to ensure that only **valid data** is persisted because they are database-agnostic, cannot be bypassed by end users, and are convenient to test and maintain. I learned a LOT about ActiveRecord [validations](https://guides.rubyonrails.org/active_record_validations.html) and [associations](https://guides.rubyonrails.org/association_basics.html) by reading through the online documentation. I feel like I have a much better grasp of those concepts now and will be more prepared for the Rails curriculum.

For the `User` model, I validated the `presence` of the user's required attributes as well as the `length` of their password and the `uniqueness` of their email address to prevent duplicate accounts from being created. I specified to the model to only check for `uniqueness` on creation because when a user tries to log in I don't want an error message saying that the email is already associated with an account because, duh it's the user's account! For the `Medication` model, I validated the `presence` of the medication's required attributes (one of which is boolean so I had to use different options for this validation). For the `Reaction` model, I validated the `presence` of the reaction's required attributes.

### Seed Data

I created three different seed files (one for each model) and saved them under `db/seeds`. I named the files `1-users.rb`, `2-medications.rb`, and `3-reactions.rb` so they would execute in numerical order. Then, in `seeds.rb` I wrote some code to iterate through `db/seeds` and load the files so that when the rake task `rake db:seed` is executed, my database is populated in exactly the order I want while at the same time keeping the seed files separated by model, making them easier to read and manipulate later.

In the first line of each seed file, I destroyed any information in the database pertaining to that file's model so I would be sure that my seeds would be persisting into a clean database. Then, I used the `.create!` method and entered my own invented data for each attribute. For the medications seeds, I repeated the above process and also assigned `created_at`, `updated_at`, and `user_id` attributes so I could associate the medication to a user upon instantiation. For the reactions seeds, I did the same but instead assigned `created_at` and `medication_id` attributes so I could associate the reaction to a medication upon instantiation.

## The Build: Controllers

The controllers are where the application configurations, routes, and controller actions are implemented. They handle all incoming requests to the app, and send back the appropriate responses to the client through the use of routes and the MVC file structure. I ended up with a total of five controllers for my app. In addition to a controller for each model, I also have a `SessionsController` and an `ApplicationController` which all of my app's other controllers use as an inheritance point.

### Application Controller

The `ApplicationController` represents an instance of my app when the server is up and running. The configure block tells the controller where to look to find the views and the public directory. It also enables the use of sessions and sets a session secret. Lastly, it registers the use of the `Sinatra Flash` gem.

My app's root route is defined in this controller as well as the route for my app's about page. I chose only these two routes to define in the `ApplicationController` because anyone can access them when they're logged out of the app.

I also defined four of my app's helper methods in the controller. Since my other controllers inherit from the `ApplicationController`, they have access to the helper methods, which means I won't have to redefine them in each controller to use them. These methods jobs are to: determine the current user, determine whether or not the current user is logged, display an error message if the current user is not logged in, and display an error message if a user is trying to view another user's content.

### Sessions Controller

The `SessionsController` is responsible for maintaining the user's session while they're using the app. The signup route will conditionally redirect the user to the home page where there will be a signup form. The input from this form will go to the `UsersController`. If the input proves valid, a new user will be created and logged in, then redirected to their dashboard. The login route will conditionally redirect the user to the home page where there will be a login form. If the input proves valid, the user will be logged in and redirected to their dashboard. The logout route clears the current session, effectively logging the current user out of the app, ending in a redirect to the home page.

### Users Controller

The `UsersController` handles all HTTP requests that call on the `User` model object. The post users route accepts information from the signup form. If the input is valid, a new user is then created based on that input and the user is logged in and redirected to their dashboard. If not, they're returned to the signup form with an error message corresponding to the invalid input. This controller also has routes to render the user dashboard page, edit the user's information, and save those edits if they are valid. In addition, the `UsersController` has its own helper method that relies on `current_user`, one of the helper methods in `ApplicationController`&mdash;a great example of the magic of inheritance at work!

### Medications Controller

The `MedicationsController` uses RESTful routes. This means that when the controller receives an HTTP request, it identifies both the HTTP verb and the URL to execute the code in the controller action that has the corresponding method and URL. The `MedicationsController` also has full CRUD actions, meaning it can create, read, update, or delete a `Medication` model object. In addition, I added a `stop-med` route which will change specific medication attributes to reflect that it is no longer current and then redirect the user to create a new reaction.

### Reactions Controller

The `ReactionsController` is much like the `MedicationsController` except it employs the use of nested URLs.

## The Build: Views

For my `layout.rb` file, I used a navbar in my header and added conditional statements as to which links I want displayed when the user is not logged in. I also used erb interpolation to create a `@title` instance variable which I set on each view page, enabling the page's title to change dynamically as the user navigates through the app.

I tried something different as well throughout my app by using partials. Sinatra is not as friendly to partials as Rails, but it worked for my purposes. I basically made view pages for my controllers to render that act as sub-layouts. Then, I used erb interpolation to render other view files within the containers I had created in the sub-layouts. This way, I could have users take multiple actions on the same page instead of navigating through the app page-by-page.

### Home Page

The home page displays a greeting with a small blurb about my app and some buttons that link to the about page, followed by the signup and login forms. Users who are already logged in will be redirected to their dashboard if they try to access the home page.

### About Page

The about page has four sections: "How To Use", "FAQs", "The Idea", and "Contact Me". Each of the first three sections has a button that will return the user either to their dashboard or the home page, depending on if they are logged in. The "How To Use" section explains the actions the user can take on each page of the app (except the forms since they're pretty self-explanatory) and each corresponding page has a question mark icon in the top right corner that will bring them to this view. The "FAQs" section is a list of  questions I imagine users may have about the app. I take the opportunity here to clarify that this app is not meant to be used as a resource for medical advice and then direct them towards a few outside resources. In "The Idea" section I talk about how I came up with the idea for my app and in the "Contact Me" section I talk a little bit about myself and post links to my social profiles and email contact.

### User Views

#### User Dashboard

Upon login or successful registration, the user will be redirected to their dashboard. The right-hand column displays the user details as entered in the signup form along with the option to edit these details by clicking the blue "Edit my user details" button. Below this is a list of the user's current medications with a few details about each medication. The user should click on the name of the medication to visit its detail page. In the left-hand column is a form to create a new medication. Upon completing the new medication form, the user will be redirected to that medication's detail page.

### Medication Views

#### Medication Detail Page

On the medication detail page the user can see all the information submitted through the new medication form. If the user left the "Usage Start Date", "Usage End Date", or "Prescribing Doctor" fields blank (as they were not required), they will not appear on the medication's detail page until they add the missing information via the "Edit Medication" button. Keep in mind that deleting a medication is different from stopping a medication. To record that a medication is no longer being taken, the user should click on the "Stop Taking Medication" button. The user should only use the "Delete Medication" button if they wish to completely remove the medication and its associated reactions from their account. Once done, this action is irreversible.

On the right-hand side underneath the medication details is a list of the user's reactions associated with this particular medication along with a few details about each reaction. The user should click on the reaction's title to visit its detail page. On the left-hand side is a form to record a new reaction. Upon completing the new reaction form, the user will be redirected to that reaction's detail page.

#### Medications List

The medications list page displays a column of the user's current medications on the left and the user's previous medications on the right followed by all of the user's medications listed at the bottom. There is also a button in the upper right-hand corner to create a new medication which when clicked will take the user to a full-page new medication form.

### Reaction Views

#### Reaction Detail Page

On the reaction detail page, the user will see the name of the medication the reaction is associated with followed by the title of the reaction. Below that will be an entry timestamp (as well as update if one was made to the reaction) and the body content of the user's recorded reaction. Following that are the dosage amount and usage instructions of the medication at the time the user recorded their reaction and whether or not the user is currently taking the medication. Further down the user will find "Edit Reaction" and "Delete Reaction" buttons as well as a button to return to the associated medication's detail page.

## Styling

If you don't know by now, you gon learn today... I LOVE HTML AND CSS!!! So I probably spent a little too much time on styling with this project if I'm going to be honest with myself (procrastination in the best way possible). I used [Bootstrap 4](https://getbootstrap.com/docs/4.3/getting-started/introduction/) for the basic layout as well as a few components like navbar, forms, and buttons. I also used [GoogleFonts](https://fonts.google.com/) for my text and [FontAwesome](https://fontawesome.com/) for the icons throughout my app.

## Troubleshooting

I had a hard time with the dates in the project. I didn't want to use the typical `yyyy-mm-dd` format for medication start and end dates because most people don't remember a specific day they started or stopped taking a medication so instead I went with `yyyy-mm`. However, Ruby did not like that at all. HTML5 allows for forms with input type `month` but Ruby will not persist the `yyyy-mm` format as a date so I appended `"-01"` on to the end of the date `params` to act as `-dd` and let Ruby persist the data. When I needed to populate the form fields for the edit forms or show the dates on any view pages, I just used the `#strftime` method to convert them back and it worked like a charm.

I had issues with the `session.user_id` not persisting as I browsed through different pages of my app. I tried to use some different gems to set a secure `session_secret` but it seemed that the only thing that would work was a plain old string. I read through the Sinatra documentation (in addition to countless other resources) but I just couldn't get the `Sysrandom` gem to work and I had wasted enough time on it already.

My biggest issue was associating the user's reaction with the correct medication. I didn't want the user to have to pick from a drop down list of their medications whenever they wanted to record a new reaction because I wanted my app to be more intuitive than that (considering the new reaction form is located on the medication's detail page). I came up with a viable solution by creating a `session.med_id` that would change depending on the last medication detail page the user visited and using the `session.med_id` to set the new reaction's `medication_id` attribute. However, another student suggested I use nested routing and that turned out to be a cleaner, much more straightforward solution to my problem. Plus, by implementing nested routes, I was able to get rid of a lot of unnecessary code that was junking up my app.

## Expansion Ideas

As I built out my app, I had a ton of ideas, many of which went in a separate file for later if I decide to expand Rxeactions and try to make it available on the market. Here are some of the ideas:

### Forms & Views

I would like to be able to add sort functions to the medications and reactions list views. I'd also like to edit the form so the user has less control over the types of input, meaning I would get back data in the exact format I want. Examples of this are requiring the medication `dosage_amount` to be integer input while using a dropdown list for the `dosage_amount` units and using a dropdown list for the `dosage_form` choices. In addition I'd like to make the forms more granular in what information they ask for, especially when it comes to the medication usage instructions.

### Additional Models

It would make much more sense to make a `Prescription` model that holds the details specific to the user and create models that act as attributes to fill in parts of the new medication form, such as medication name, dosage, and dosage form. This could even apply to the prescribing doctor attribute as I also had the idea to make a `Doctor` model. The doctor would be able to view and edit medications with which they're associated as well as view the associated reactions and user information, but they will not have edit privileges and they also would not be able to view any of the user's other medications unless they were listed as the prescribing doctor for those too. The doctor would also have contact info properties that would be viewable by all users in case any of their patients (or prospective patients) needed to get in touch with them. Another user model idea would be to add a family function in which users could be grouped by families, allowing each other viewing permissions, or a user could track a dependent's medications as a sub-user under their own account.

### Medication Information

I would like to add an alarm feature to remind people to take their medication and then have a box for them to check off when they actually take it so they don't forget if they did or not. If the app could track the quantity of the medication the user has compared with the dosage amount and usage instructions, it would be able to send the user a reminder when they need to order a refill&mdash;or even send a reminder to the pharmacy or prescribing doctor. It would also be beneficial to the users if they could access verified medical advice about their specific medications right from the medication details page. I'd really like to be able to add the capability for an alert to appear on the user's dashboard when they list two medications as `current` that have been shown to present negative interactions with each other. Or possibly an alert for common or dangerous side effects when a user adds a new medication.

### Overall Reactions

I'm thinking about how I could implement a text analyzer to comb the titles and content of the reactions for each medication and give an overall rating for the medication based on the user's recorded reactions. It would decide if the medication was mostly helpful to the user, mostly harmful to the user, or maybe neither and display this analysis on the medication's details page.

## Project Demo Video

<iframe width="100%" frameborder="0" src="https://www.youtube.com/embed/xRKItzdbQwk" allowfullscreen></iframe>
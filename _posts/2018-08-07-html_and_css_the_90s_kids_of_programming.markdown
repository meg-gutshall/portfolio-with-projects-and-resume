---
layout: post
title: "HTML & CSS: The ‘90s Kids of Programming"
date: 2018-08-07 00:12:12 -0400
permalink: html-and-css-the-90s-kids-of-programming
featured-img: "/static/img/html-css.png"
categories: [Flatiron School]
tags: [software engineering, HTML, CSS]
excerpt: <p>The combination of HTML and CSS make up a huge part of front-end web development. If a program touches the Internet, it will without a doubt rely on HTML and CSS at its base level. There are plenty of frameworks and templates that do a lot of (if not all) the work for you, but this leads to websites and apps looking like homogenous cookie-cutter builds. Contrary to popular belief, I think that HTML and CSS are still as important as they were some two decades past so in this post I go back to the very basics and give an introduction to each of these amazing languages for those who may be new to software development.</p>
---

_For brevity’s sake, I am going to use acronyms throughout this post. Full phrases and relevant information will be covered at the end of this post._

After almost two months in the [online software engineering program](https://flatironschool.com/career-courses/coding-bootcamp/online/), I finally feel like I’m getting into the thick of the curriculum at Flatiron. I’ve finished Intro to Ruby Development, learned about Git and [GitHub](https://github.com/meg-gutshall), sped through Procedural Ruby, and am now working on the Object Oriented Ruby section. One section I especially enjoyed though was HTML and CSS Introduction.

![HTML & CSS Symbols](/img/post-images/html-css.png)

## HTML: The Language of the Web

HTML is the language of the web, however, it is <ins>not</ins> a programming language, but rather a _markup_ language. HTML documents are comprised of content (text written in plain English) and markup (HTML tags meant to structure the content). These documents are used by web servers and web browsers to display the non-markup version of the document&mdash;a web page&mdash;in a process called rendering.

Here’s a basic overview of the rendering process: HTML documents are stored at unique locations (IP addresses) on web servers and made available to web browsers via the Internet. A user will type a URL into a web browser’s search bar and the web browser feeds the URL into the DNS, which then translates the URL address to its corresponding IP address. The web browser then uses HTTP to communicate with the server at the returned IP address and requests the sought-after information&mdash;the HTML files used to render the web page. Once the web browser receives these files, it translates the HTML code and displays the web page on our screen.

## The Semantics of HTML

As a linguistics nerd (in case you missed it, there's a great example of my Spanish linguistics nerdiness coming out in my last post, ["The Little Things Matter"](https://meghangutshall.com/my-blog/the-little-things-matter)), I love HTML due to its semantic capabilities, so bear with me as I geek out for a minute…

An HTML document consists of two sections: a head and a body. The head is not displayed as part of the web page when the browser renders the HTML document. Instead it contains data (such as page title, a favicon, links to the document’s CSS and JavaScript, and metadata) which relays additional information not contained in the body section to the web browser. A web page’s title and favicon appear in the window tab of the web browser. The CSS link provides styling rules for the HTML document and the JavaScript link enables additional, more complex features. Metadata defines the document’s language, character encoding, author, and description. Adding an accurate description that includes applicable keywords is especially important as it affects the site’s SEO and will determine the frequency with which the web page appears in Internet search results.

The meat of the document, the body, is composed of HTML elements. An HTML element contains an opening tag, a section of content, and a closing tag*. The type of tag used in the element determines the meaning and structure (semantics!) of the enclosed content. Common HTML tags denote links, line breaks, headings, images, lists, paragraphs, and tables. Web browsers read the HTML tags and apply their meaning to the enclosed content when rendering a web page. HTML elements can also accept attributes to transfer more detailed information regarding the enclosed content. It is important to assign HTML tags accurately, not only to structure the web page correctly, but also to enable website accessibility for those who use screen readers and other tools to navigate the web.

## CSS: The Beauty to HTML’s Beast

CSS styles web pages by assigning a CSS selector to an HTML element and a declaration to the selector. The most commonly used CSS selectors point toward an HTML element, class, or id, but can also call on an element’s attribute or its relationship with the surrounding elements. CSS declarations are made up of properties and their corresponding property values. Multiple CSS declarations can apply to one or more CSS selectors–called a declaration block–and multiple CSS selectors can apply to one or more HTML elements, giving CSS a totally customizable execution.

A general convention among programmers is to completely separate the tasks of writing content (HTML) and styling content (CSS). Although CSS was designed specifically to work with HTML, in most cases a site’s CSS is located in an entirely separate document than the HTML. Instead, the HTML document stores a link to its corresponding CSS document in the head section which the web browser calls on to style the web page at the time of rendering.

## Why HTML and CSS Are Important for Programmers

The combination of these two languages make up a huge part of front-end web development. As a software engineer, you’ll need to be able to read and write HTML to code and debug your programs... and you’ll want to be familiar with CSS to make your programs pretty! While on Flatiron’s learning platform, the message that stood out to be the most in the HTML and CSS Introduction section was: **All pathways to web programming success rely on strength in HTML.** And as I see it, the first step on that pathway is your [portfolio website](https://meghangutshall.com/). This cannot be created without HTML and will most likely look awful without CSS. Your website is your prospective employers’ first impression of you as a programmer&mdash;so make it a good one!

There is so much information available on HTML and CSS as they are fundamental programming tools and, simply, because they have been around for a very long time. I could write a dozen more blog posts on the topic but, for now, will leave you with a short index of the aforementioned acronyms as well as some of my favorite resources regarding HTML and CSS.

### Index

CSS → Cascading Style Sheets<br>
DNS → Domain Name Server<br>
HTML → Hypertext Markup Language<br>
HTTP → Hypertext Transfer Protocol<br>
SEO → Search Engine Optimization<br>
URL → Uniform Resource Locator<br>

### Resources

[Mozilla Development Network](https://developer.mozilla.org/en-US/docs/Learn/HTML)<br>
[Learn to Code HTML & CSS](https://learn.shayhowe.com/html-css/)<br>
[CSS Tricks](https://css-tricks.com/guides/)<br>
[W3Schools](https://www.w3schools.com/)

_*Not all HTML elements require a closing tag (such as `img`, `input`, `br`, `hr`, `meta`, and a few others), but that's a topic for another blog post. When in doubt, use a closing tag._

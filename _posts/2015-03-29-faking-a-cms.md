---
layout: post
title:  "Faking a CMS with Tabletop.js & Handlebars.js"
date:   2015-03-20
categories: CMS, javascript, templating
excerpt: Each week, UrbanOutfitters.com publishes over a hundred new marketing messages across its homepage and gateway pages. And believe it or not, there's no front-end CMS for it! For years, all of that marketing content &mdash; copy, links, images &mdash; has been hardcoded by hand in html files. So here's the story of how I came up with a makeshift CMS!
---


If you look under the hood of the Urban Outfitters Website, you'll see a combination of enterprise Java, AngularJS, xml, php, python, Sass, node modules, and a whole slue of other JavaScript libraries. Some of the codebase is ancient, and some of it is real new and fancy &mdash; production ES6 code is actually just a few weeks away.

All content pages (the homepage and gateways for example) are static html includes, while category and product pages are dynamic. I was hired to update the weekly content on the content pages, and my job entailed a whole lot of copying & pasting. I quickly learned to master keyboard shortcuts in Sublime (I owe my life to Command + d)! And I realized that I coudn't singlehandedly change a huge machine and rearchitect such a significant part of the site, so I had to come up with a way to work within my means to automate the copy/pasting from an excel spreadsheet to html file. 

My first pass at automation was getting my fella [Steve](http://www.twitter.com/stevetlamb) to help me write python and shell scripts to pull data from exported .csv files and drop it into a Jinja template, which was basically a loop with some conditional logic, to churn out HTML. It worked for a time (almost a year) in automating the bulk of the content coding, but only worked well for a specific page layout. It also required some awkward workflow steps, thanks to Microsoft Office's mysterious special characters.

Designers wanted more flexibilty and variation in homepage and gateway layouts, which meant coming up with a new system. Moved to a google doc so could use Tabletop.js to turn data into a giant json object. and moved from jinja templates that was a big single loop to a handlebars template mapped to specific points in the json object.  

I used handlebars because i wanted to give it a try, but if i were to start over again, i'd use nunjucks as a template because it handles template logic a lot better than handlebars.  And handlebars documentation is not indexed well, so it can be annoying to find what you're looking for. 


--make sure to delete extra empty rows- first time we ran it, there were thousands of empty cells and it rendered so slow that it seemed like something was broken. 






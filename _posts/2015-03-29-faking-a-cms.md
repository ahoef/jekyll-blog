---
layout: post
title:  "Faking a CMS with Tabletop.js & Handlebars.js"
date:   2015-03-20
categories: CMS, javascript, templating
excerpt: Each week, UrbanOutfitters.com publishes over a hundred new marketing messages across its homepage and gateway pages. And believe it or not, there's no front-end CMS for it! For years, all of that marketing content &mdash; copy, links, images &mdash; has been hardcoded by hand in html files. So here's the story of how I faked a CMS!
---


If you look under the hood of the Urban Outfitters Website, you'll see a combination of enterprise Java, AngularJS, xml, php, python, Sass, node modules, and a whole slue of other javascript libraries. Some of the codebase is ancient, and some of it is real new and fancy- production ES6 code is actually just a few weeks away.

All content pages (the homepage and gateways for example) are static html includes, while category and product pages are dynamic. I was hired to update the weekly content on the content pages, and it was a lot of manual labor. It taught me to master keyboard shortcuts in Sublime. I realized that I coudn't singlehandedly change a huge machine and rearchitect the whole site, so i had to come up with a way to work within my means to automate the copy/pasting from an excel spreadsheet to html file. 

First pass was gettign Steve Lamb to help me make python scripts to pull data from exported csvs and drop it into Jinja templates to spit out html. 

It worked for a time (almost a year) in automating the bulk of the work, but only worked for a specific gateway layout, and required some awkward workflow steps. 

Designers wanted more flexibilty and variation in page layout, so i came up with a new system. Moved to a google doc so could use Tabletop.js to turn data into a giant json object. and moved from jinja templates that was a big single loop to a handlebars template mapped to specific points in the json object.  

I used handlebars because i wanted to give it a try, but if i were to start over again, i'd use nunjucks as a template because it handles template logic a lot better than handlebars.  And handlebars documentation is not indexed well, so it can be annoying to find what you're looking for. 


--make sure to delete extra empty rows- first time we ran it, there were thousands of empty cells and it rendered so slow that it seemed like something was broken. 






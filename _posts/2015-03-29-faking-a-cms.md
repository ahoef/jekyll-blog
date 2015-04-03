---
layout: post
title:  "Faking a CMS with Tabletop.js & Handlebars.js"
date:   2015-03-20
categories: CMS, javascript, templating
excerpt: Each week, UrbanOutfitters.com publishes over a hundred new marketing messages across its homepage and gateway pages. And believe it or not, there's no front-end CMS for it! For years, all of that marketing content &mdash; copy, links, images &mdash; has been hardcoded by hand in html files. So here's the story of how I came up with a makeshift CMS!
image: "/images/uo.png"
---

<img src="/images/uo.png">

### The Problem ###

If you look under the hood of the Urban Outfitters Website, you'll see a combination of enterprise Java, AngularJS, xml, php, python, Sass, node modules, and a whole slue of other JavaScript libraries. Some of the codebase is ancient, and some of it is real new and fancy &mdash; production ES6 code is actually just a few weeks away.

All content pages (the homepage and gateways for example) are static html includes, while category and product pages are dynamic. I was hired to update the weekly content on the content pages, and my job entailed a whole lot of copying & pasting. I quickly learned to master keyboard shortcuts in Sublime (I owe my life to Command + d)! And I realized that I coudn't singlehandedly change a huge machine and rearchitect such a significant part of the site, so I had to come up with a way to work within my means to automate the copy/pasting from an excel spreadsheet to html file. 

### The Solution ###

My first pass at automation was getting my fella [Steve](http://www.twitter.com/stevetlamb) to help me write python and shell scripts to pull data from exported .csv files and drop it into a Jinja template, which was basically a loop with some conditional logic, to churn out HTML. Check out [the code](https://github.com/ahoef/uo-marketing) if you want! It worked for a time (almost a year) in automating the bulk of the content coding, but only worked well for a specific page layout. It also required some awkward workflow steps, thanks to Microsoft Office's mysterious special characters.

Designers wanted more flexibilty and variation in homepage and gateway layouts, which meant coming up with a new system. My boss convinced marketing folks to move from excel to a Google spreadsheet so that we could use [Tabletop.js](https://github.com/jsoma/tabletop) to turn the data into a giant json object. We're using the Tabletop/Google Doc approach for the site navigation, and it's been working out pretty well. The only drawback is that Tabletop requires a public spreadsheet url (at least at the time of this writing), so the doc has to be published to the web as a read-only file.

Tabletop.js integrates nicely with [Handlebars.js](http://handlebarsjs.com/), so I also moved from a single loop template in Jinja to Handlebars templates mapped to specific points in the Tabletop Object. I quickly learned that the Handlebars documentation is not indexed or searchable, and that template logic in Handlebars is kind of a pain, so if I were to start over again, I'd definitely use another templating engine, like [Nunjucks](https://mozilla.github.io/nunjucks/) or [Underscore](http://underscorejs.org/). Separating markup and functionality by limiting template logic makes sense, but at the end of the day it's less code and less headache to write a for loop within a template by declaring a one-line expression.



So the fake CMS that I came up with allows the marketing team to fill out various tabs on a spreadsheet with their weekly updates, and allows our developers to copy over large chunks of generated markup, instead of copying over cell by cell.   

### The Lessons Learned ###

--make sure to delete extra empty rows- first time we ran it, there were thousands of empty cells and it rendered so slow that it seemed like something was broken. 

https://github.com/ahoef/handlebars-tabletop-demo




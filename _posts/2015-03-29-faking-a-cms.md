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

All content pages (the homepage and gateways for example) are static html includes, while category and product pages are dynamic. I was hired to update the weekly content on the content pages, and my job entailed a whole lot of copying & pasting. I quickly learned to master keyboard shortcuts in Sublime (I owe a lot to Command + D)! And I realized that I coudn't singlehandedly change a chugging machine and rearchitect such a significant part of the site, so I started thinking of ways for me and my fellow junior developers to work within our means to automate the copy/pasting from an Excel spreadsheet to html file. 

### The Solution ###

My first pass at automation was getting [Steve](http://www.twitter.com/stevetlamb) to help me write python and shell scripts to pull data from exported .csv files and drop it into a Jinja template, which was basically a loop with some conditional logic, to churn out HTML. Check out [the code](https://github.com/ahoef/uo-marketing) if you want! It worked for a time (almost a year) in automating the bulk of the content coding, but only worked well for a specific page layout. It also required some awkward workflow steps, due to Microsoft Office's mysterious special characters.

Designers wanted more flexibilty and variation in homepage and gateway layouts, which meant coming up with a new system. My boss convinced marketing folks to move from Dxcel to a Google spreadsheet so that we could use [Tabletop.js](https://github.com/jsoma/tabletop) to turn the data into a giant json object. We're using the Tabletop/Google Doc approach for the site navigation, and it's been working out pretty well. The only drawback is that Tabletop requires a public spreadsheet url (at least at the time of this writing), so the doc has to be published to the web as a read-only file.

Tabletop.js integrates nicely with [Handlebars.js](http://handlebarsjs.com/), so I also moved from a single loop template in Jinja to Handlebars templates mapped to specific points in the Tabletop Object. I quickly learned that the Handlebars documentation is not indexed or searchable, and that template logic in Handlebars is kind of a pain, so if I were to start over again, I'd definitely use another templating engine, like [Nunjucks](https://mozilla.github.io/nunjucks/) or [Underscore](http://underscorejs.org/). Separating markup and functionality by limiting template logic makes sense, but at the end of the day it's less code and less headache to write a for loop within a template by declaring a one-line expression.

So in a nutshell, the new code I came up with works by grabbing the data on all of the spreadsheet tabs, turning it into a json object, and feeding that object to templates so they can generate code based on parameters passed into the rendering function. Apart from the templates themselves, here's the function that does the heavy lifting:

{% highlight JavaScript %}
function setTemplate(gateway, templateVersion, yymmdd, cmWeek, tagStyle) {

	var template = Handlebars.templates[templateVersion],
		context = tabletop.sheets(gateway),
		weeklyInfo = {yymmdd: yymmdd, cmWeek: cmWeek, tagStyle: tagStyle, gateway: gateway},
		newContext = $.extend({}, context, weeklyInfo),
		html = template(newContext),
		$gwSelector = $('#' + gateway);

	$gwSelector.append(html);
	console.log($gwSelector.html());

}
{% endhighlight %}

It doesn't make a ton of sense out of context, but the end result is html that's logged out in the console when the function is called with its weekly variations of parameters. One of the coolest parts is the $.extend method that combines the object for a spreadsheet tab with another object that includes additional data the templates need. It allows us to reference the spreadsheet data object like this:

<code class="single-line">
&#123;&#123; elements.0.TAG1 &#125;&#125;
</code>

And the additional parameter data as a top level object like this:  

<code class="single-line">
&#123;&#123; yymmdd &#125;&#125;
</code> 


I'm thinking of this project as a fake CMS because it allows the marketing team to fill out various tabs on a spreadsheet with their weekly updates, and it pumps out browser-ready code, saving developers from having to manually update individual links, copy, and image paths for hundreds of marketing modules. Now that UO is about to launch a Canadian site in both French and English, the amount of marketing content will triple. But by tweaking the templates a bit, we'll be able to handle the volume!   

### The Lessons Learned ###

-There are lots of directions this can go in the future! It would awesome to pass over the step of having to copy the chunk of generated code from the browser's console into an html file, and rather generate the actual html files themselves. A former Urban developer actually wrote a node module to use Grunt to compile Handlebars templates to html files, which is definitely worth further exploration.

-Special characters can still sneak into Google Docs! Excel is a hundred times worse, but Google is not immune to weird, invisible characters that mysteriously botch things up. 

-It turns out that thousands of empty cells in the spreadsheet sloooooooows down the application in a real way. When I was building this, there were a number of times when I thought something was broken because code was not showing up in the console, but it turned out that the template was just taking a noticeably long time to render.

https://github.com/ahoef/handlebars-tabletop-demo




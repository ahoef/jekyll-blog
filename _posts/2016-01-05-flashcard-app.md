---
layout: post
title:  "Flashcard App"
date:   2016-01-05
categories:
excerpt:  
image: "/images/.png"
---

<img src="/images/.png">


Last month I started a new project at work, the magnitude of which is greater than anything else I've worked on in the past. One of the first and, arguably most important, steps in tackling this enormous project was to decide on a tech stack that best meets the business' needs as a global e-commerce company. So in recent weeks, I, along with a team of other developers at URBN, have had the opportunity to take several languages and frameworks for a test drive to evaluate pros and cons.

In this spirit of exploration, I worked on a few of my own JavaScript framework tech spikes outside of work - I built the same web app in vanilla ES2015, React & Redux, and Angular2. It's a basic flashcard app, where users can paginate forward and backward through cards, and click to reveal answers. I had a lot of fun cooking up some fancy Sass mixins, but the real gain in this project was thinking about the same data modeling in three different ways. 

###Version #1: JavaScript Flashcards, built with vanilla ES2015 & jQuery###

<!-- There are a number of github repos that get you started with a Gulp, Babel, and Browserify build configuration, like this one- codeschool -->

###Version #2: Photography Flashcards, built with React & Redux###

Back in August 2015, Henrik Joreteg [wrote about Redux](https://blog.andyet.com/2015/08/06/what-the-flux-lets-redux/) when it was just barely a thing. Even as such, he described Redux as a gamechanger in the JavaScript world, to the likes of jQuery, Backbone, and Node when they first emerged. And Joreteg is among a large group of Redux believers taking the internet by storm.  

Joreteg's main argument for hyping Redux is that state management in single-page applications 'is hard', and that with MVC patterns 'It’s still very easy to make a mess of your app.' Redux offers an opinionated, functional-based solution to this state problem by using a single store for your tree of observable objects, as opposed to patterns that use multiple stores, like Flux. You can think of this single store as a function that takes an action and a state as arguments, and returns a new state. So there aren't competing features in your application that claim a stake in your state; they all have to answer to Redux, the state gatekeeper. 

So in theory, this is good. But building a web app with [Redux](https://github.com/rackt/redux), paired with React, which is also very opinionated, was a lot harder than I expected. I don't think that the hurdles for this version of my flashcard app came from Redux though. They came from React. This was my first time using React, and it turns out that I hate it! I really should do a deeper dive into it to see why so many developers love it, and why so many companies are investing in it, because to me, it just seems like React what you get if you were to scramble up all of your HTML, CSS, JavaScript files into one, and add in a whole lot of strange syntax and 'props'. 

###Version #3: Vocabulary Flashcards, built with Angular 2 & TypeScript###

After what felt like a long time coming, [Angular 2](https://angular.io/), a ground-up rewrite of Angular 1.x, was recently released in beta! This new version gets rid of a lot of cruft (goodbye <code>ng-</code>vomit all over the rendered DOM) and booby traps (goodbye <code>$scope</code>), while still enabling modularity and robust view logic. 

By working on this flashcard app, I found differences in 3 main areas in Angular 1 & 2:
	- Build process
	- Components
	- Template syntax

Build Process

With earlier versions of Angular, all you needed was a script tag reference to the Angular CDN and an html file to get started. With Angular 2, if you follow the recommended development workflow, you'll have to compile [TypeScript](http://www.typescriptlang.org/) down to JavaScript, and ES6 down to ES5, if you choose to use it. But these days it seems impossible to escape a JS build process. ¯\_(ツ)_/¯

The [Angular quickstart demo](https://angular.io/docs/ts/latest/quickstart.html) gets you up and running with a build pretty quickly and painlessly.  

Components vs Controllers & Directives

Components take the place of controllers and directives together; they're what do the heavy lifting in an app [This gist](https://gist.github.com/ahoef/df91b536575e50531593) demonstrates controllers vs components 

Template Syntax

...

###In a nutshell###

Ultimately I liked making the Angular 2 flashcard app the best! I found that out of the three, it was the quickest to get up and running, and the most enjoyable to build. Angular 2 is still in beta though, so adopting it for high-profile production sites right now could be hairy. 

I'm going to run for the hills if someone tries to get me to write React code (although Redux is huge part of my life now, since we ended up opting for Redux as a key player in this project at work). 

And while I really like using ES2015 syntax in my code, I can't see myself ever opting to build a large-scale MVC framework with vanilla JS, as the view layer can get very unweildy very fast. 




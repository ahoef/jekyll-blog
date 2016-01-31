---
layout: post
title:  "Three Implementations of a Flashcard App"
date:   2016-01-05
categories:
excerpt: "I recently built the same web app in vanilla ES2015, React & Redux, and Angular 2. It's a basic flashcard app, where users can paginate forward and backward through cards, and click to reveal answers. I had a lot of fun cooking up some fancy Sass mixins, but the real gain in this project was thinking about the same data modeling in three different ways." 
image: "/images/flashcard-app.png"
---

Last month I started a new project at work, the magnitude of which is greater than anything else I've worked on in the past! One of the first and most important steps in tackling this huge project was to decide on a tech stack that best meets the business' needs as a global e-commerce company. So in recent weeks, I, along with a team of other developers at URBN, have had the opportunity to take several languages and frameworks for a test drive to evaluate pros and cons.

In this spirit of exploration, I worked on a few of my own JavaScript framework prototypes outside of work - I built the same web app in vanilla ES2015, React & Redux, and Angular 2. It's a basic flashcard app, where users can paginate forward and backward through cards, and click to reveal answers. I had a lot of fun cooking up some fancy Sass mixins, but the real gain in this project was thinking about the same data modeling in three different ways. 

<img src="/images/flashcard-app.png">

<br>
<br>

###Version 1: [JavaScript Flashcards](http://www.ahoef.co/js-flashcards), built with vanilla ES2015 & jQuery###

For this version I used Babel & Browserify to transpile JavaScript so that I could use ES2015 syntax and features such as import/export statements, classes, promises, template strings, & arrow functions.  

There are a number of starter repos for build setups, but I started with Code School's [babel-with-gulp](https://github.com/codeschool/babel-with-gulp) repo to avoid some potential headaches with getting code to transpile. The gulpfile in that repo only builds JS, so I added in functionality to watch JS files and also to compile & watch Sass files. From there I started building out a basic MVC pattern, where a controller's promise chain calls model methods and ultimately renders out the view. 

Out of the 3 flashcard apps, this one has by far the smallest JS file size. The large majority of the total file size comes from jQuery (86K), and I could easily trim that out by using native selectors & event handling. 

Compiled JS: 100K <br>
Source Code: [https://github.com/ahoef/es2015-flashcard-app](https://github.com/ahoef/es2015-flashcard-app)
<br>
<br>


###Version 2: [Photography Flashcards](http://ahoef.co/photo-flashcards), built with React & Redux###

Back in August 2015, Henrik Joreteg [wrote about Redux](https://blog.andyet.com/2015/08/06/what-the-flux-lets-redux/) when it was just barely a thing. Even as such, he described Redux as a gamechanger in the JavaScript world, to the likes of jQuery, Backbone, and Node when they first emerged. And Joreteg is among a large group of Redux believers taking the internet by storm.  

Joreteg's main argument for hyping Redux is that state management in single-page applications 'is hard', and that with MVC patterns 'It’s still very easy to make a mess of your app.' Redux offers an opinionated, functional-based solution to this state problem by using a single store for your tree of observable objects, as opposed to patterns that use multiple stores, like Flux. You can think of this single store as a function that takes an action and a state as arguments, and returns a new state. So there aren't competing features in your application that claim a stake in your state; they all have to answer to Redux, the state gatekeeper. 

So in theory, this is good. But building a web app with [Redux](https://github.com/rackt/redux), paired with React, which is also very opinionated, was a lot harder than I expected. I don't think that the hurdles came from Redux though; they came from React. This was my first time using React, and I have a lot more to learn. I found myself struggling with the scrambled HTML, JS, and CSS, but I do see the advantages of standalone components. 

Because this version of the app required more dependencies (React, ReactDOM, Redux), the final JS file size ended up being pretty large.  

Compiled JS: 723K <br>
Source Code: [https://github.com/ahoef/react-redux-flashcard-app](https://github.com/ahoef/react-redux-flashcard-app)



###Version 3: [Vocabulary Flashcards](http://www.ahoef.co/vocab-flashcards), built with Angular 2 & TypeScript###

After what felt like a long time coming, [Angular 2](https://angular.io/), a ground-up rewrite of Angular 1.x, was recently released in beta! This new version gets rid of a lot of cruft (goodbye <code>ng-</code>vomit all over the rendered DOM) and booby traps (goodbye <code>$scope</code>), while still enabling modularity and robust view logic. 

By working on this flashcard app, I found differences in 3 main areas in Angular 1 & 2:
* Build process
* Components
* Template syntax

####Build Process

With earlier versions of Angular, all you needed was a script tag reference to the Angular CDN and an html file to get started. With Angular 2, if you follow the recommended development workflow, you'll have to compile [TypeScript](http://www.typescriptlang.org/) down to JavaScript, and ES6 down to ES5, if you choose to use it. But these days it seems impossible to escape a JS build process. ¯\_(ツ)_/¯

The [Angular quickstart demo](https://angular.io/docs/ts/latest/quickstart.html) gets you up and running with a build pretty quickly and painlessly.  

####Components vs Controllers

Components take the place of controllers and directives together; they're what do the heavy lifting in an app. 

Here's a basic controller in Angular <1.5:

<code>

//controller
angular.module('foo')
	.controller('myMessage', function($scope) {
		$scope.message = 'Here is a message for you!';
	});
<br>
&lt;div ng-controller="myMessage"&gt;
	{{ message }}
&lt;/div&gt;

</code>

And here's how you'd write a basic component in Angular 2:

<code>

import { Component, View } from 'angular2/angular2';
<br>
@Component({
  selector: 'my-app'
})
@View({
  template: 'Here is a message for you!'
})
<br>
export class myMessage {}

</code>

####Template Syntax

There are quite a few [template syntax updates](https://angular.io/docs/ts/latest/guide/template-syntax.html) with A2, but here's a list of the ones I encountered while writing my app:

* `ngFor` replaces `ng-repeat`
* `ng-show`, `ng-hide`, & `ng-if` are collapsed into `ngIf`
* `ngFor`, `ngIf`, and `ngSwitch` use the `*` prefix
* iterators/template variables use the `#` prefix
	* Ex: `*ngFor="#card of flashcards"`

The initial load of this version of the flashcard app is noticeably slower than the rest, due to a significantly larger request payload. There a number of dependencies required to get Angular 2 running in current browsers, which really skyrocket end file size.   

Compiled JS: 1.7MB

Source Code: [https://github.com/ahoef/angular2-flashcard-app](https://github.com/ahoef/angular2-flashcard-app)



###In a nutshell###

Ultimately I liked making the Angular 2 flashcard app the best! I found that out of the three, it was the quickest to get up and running, and the most enjoyable to build. Angular 2 is still in beta though, so adopting it for high-profile production sites right now could be hairy, especially because the required polyfills. 

I might run for the hills if someone asks me to write React code. JK! I have a lot more to learn with React, and I look forward to it. Redux is huge part of my life now, since we ended up opting for Redux as a key player in this project at work. We've learned that out-of-the-box Redux does not handle async functions all that well, so we've implemented [sagas](https://github.com/yelouafi/redux-saga) for a generator-based approach. 

And while I really like using ES2015 syntax and features in my code, I can't see myself ever opting to build a large-scale MVC framework with vanilla JS, as the view layer can get very unweildy very fast. 

That's all for now... next up is a flashcard app in [Elm](http://elm-lang.org/)! Stay tuned! 




---
layout: post
title:  "Three Implementations of a Flashcard App"
date:   2016-01-05
categories:
excerpt: "To see the differences between frameworks, I recently built the same web app in vanilla ES2015, React & Redux, and Angular 2. It's a basic flashcard app, where users can paginate forward and backward through cards, and click to reveal answers. I had a lot of fun cooking up some fancy Sass mixins, but the real gain in this project was thinking about the same data modeling in three different ways." 
image: "/images/flashcard-app.png"
---

Last month I started a new project at work, the magnitude of which is greater than anything else I've worked on in the past! One of the first and most important steps in tackling this huge project was to decide on a tech stack that best meets the business' needs as a global e-commerce company. So in recent weeks, I, along with a team of other developers at URBN, have had the opportunity to take several languages and frameworks for a test drive to evaluate pros and cons.

In this spirit of exploration, I worked on a few of my own JavaScript framework prototypes outside of work - I built the same web app in vanilla ES2015, React & Redux, and Angular 2. It's a basic flashcard app, where users can paginate forward and backward through cards, and click to reveal answers. I had a lot of fun cooking up some fancy Sass mixins, but the real gain in this project was thinking about the same data modeling in three different ways. 

<img src="/images/flashcard-app.png">

<br>

###Version 1: [JavaScript Flashcards](http://www.ahoef.co/js-flashcards), built with vanilla ES2015 & jQuery###

For this version I used Babel & Browserify to transpile JavaScript so that I could use ES2015 syntax and features such as import/export statements, classes, promises, template strings, & arrow functions.  

There are a number of starter repos for build setups, but I started with Code School's [babel-with-gulp](https://github.com/codeschool/babel-with-gulp) repo to avoid some potential headaches with getting code to transpile. The gulpfile in that repo only builds JS, so I added in functionality to watch JS files and also to compile & watch Sass files. From there I started building out a basic MVC pattern, where a controller's promise chain calls model methods and ultimately renders out the view. This is what the <code>.render</code> method looks like:

    render(model, action){
        model.generateRandomNum()
            .then( number => {
                return model.addIndexToOrderArray(number, action);
            })
            .then( data => {
                return model.attachContentToDOM(action);
            })
            .catch( error => {
                console.log(error);
            })
    } 

Out of the 3 flashcard apps, this one has by far the smallest JS file size. The large majority of the total file size comes from jQuery (86K), and I could easily trim that out by using native selectors & event handling. 

**Compiled JS:** 100K <br>
**Source Code:** [https://github.com/ahoef/es2015-flashcard-app](https://github.com/ahoef/es2015-flashcard-app)
<br>
<br>


###Version 2: [Photography Flashcards](http://ahoef.co/photo-flashcards), built with React & Redux###

Back in August 2015, Henrik Joreteg [wrote about Redux](https://blog.andyet.com/2015/08/06/what-the-flux-lets-redux/) when it was just barely a thing. Even as such, he described Redux as a gamechanger in the JavaScript world, to the likes of jQuery, Backbone, and Node when they first emerged. And Joreteg is among a large group of Redux believers taking the internet by storm.  

Joreteg's main argument for hyping Redux is that state management in single-page applications 'is hard', and that with MVC patterns 'It’s still very easy to make a mess of your app.' Redux offers an opinionated, functional-based solution to this state problem by using a single store for your tree of observable objects, as opposed to patterns that use multiple stores, like Flux. You can think of this single store as a function that takes an action and a state as arguments, and returns a new state via a reducer function. 

Here's a basic action from my app. Per Redux conventions, an action is just a function that returns an object that describes a state change:

	export function showAnswer() {
	    return { 
	        type: 'SHOW_ANSWER'
	    }
	}

And here's the app reducer, which takes the action and current state, and returns a new state object:

	function changeState(state = initialState, action) {
	    switch (action.type) {
	        case 'SHOW_ANSWER':
	            return Object.assign({}, state, {
	                answerHidden: false,
	                nextCard: false
	            })
	        case 'NEXT_CARD':
	            return Object.assign({}, state, {
	                answerHidden: true,
	                nextCard: true
	            })
	        default:
	            return state;
	    }
	}


So there aren't competing features in your application that claim a stake in your state; they all have to answer to Redux, the gatekeeper of a single state. In theory, this is good. But building a web app with [Redux](https://github.com/rackt/redux), paired with React, which is also very opinionated, was a lot harder than I expected. I don't think that the hurdles came from Redux though; they came from React. This was my first time using React, and I have a lot more to learn. I found myself struggling with the scrambled HTML, JS, and CSS, but I do see the advantages of standalone components. 

Because this version of the app required more dependencies (React, ReactDOM, Redux), the final JS file size ended up being large.  

**Compiled JS:** 723K <br>
**Source Code:** [https://github.com/ahoef/react-redux-flashcard-app](https://github.com/ahoef/react-redux-flashcard-app)
<br>
<br>


###Version 3: [Vocabulary Flashcards](http://www.ahoef.co/vocab-flashcards), built with Angular 2 & TypeScript###

After what felt like a long time coming, [Angular 2](https://angular.io/), a ground-up rewrite of Angular 1.x, was recently released in beta! This new version gets rid of a lot of cruft (goodbye <code>ng-</code>vomit all over the rendered DOM) and booby traps (goodbye <code>$scope</code>), while still enabling modularity and robust view logic. 

By working on this flashcard app, I found differences in 3 main areas in Angular 1 & 2:
<ol>
	<li>Build process</li>
	<li>Components</li>
	<li>Template syntax</li>
</ol>

####1. Build Process

With earlier versions of Angular, all you needed was a script tag reference to the Angular CDN and an html file to get started. With Angular 2, if you follow the recommended development workflow, you'll have to compile [TypeScript](http://www.typescriptlang.org/) down to JavaScript, and ES6 down to ES5, if you choose to use it. But these days it seems impossible to escape a JS build process. ¯\&#95;(ツ)&#95;/¯

The [Angular quickstart demo](https://angular.io/docs/ts/latest/quickstart.html) gets you up and running with a build pretty quickly and painlessly.  

####2. Components vs Controllers

Components take the place of controllers and directives together; they're what do the heavy lifting in an app. 

Here's a basic controller in Angular <1.5:

	//controller
	angular.module('foo')
		.controller('myMessage', function($scope) {
			$scope.message = 'Here is a message for you!';
		});

	//view
	<div ng-controller="myMessage">
		{ { message } }
	</div>

And here's how you'd write a basic component in Angular 2:

	import { Component, View } from 'angular2/angular2';
	
	@Component({
	  selector: 'my-app'
	})
	@View({
	  template: 'Here is a message for you!'
	})
	
	export class myMessage {}


####3. Template Syntax

There are quite a few [template syntax updates](https://angular.io/docs/ts/latest/guide/template-syntax.html) with A2, but here's a list of the ones I encountered while writing my app:

* `ngFor` replaces `ng-repeat`
* `ng-show`, `ng-hide`, & `ng-if` are collapsed into `ngIf`
* `ngFor`, `ngIf`, and `ngSwitch` use the `*` prefix
* iterators/template variables use the `#` prefix
	* Ex: `*ngFor="#card of flashcards"`


The initial load of this version of the flashcard app is noticeably slower than the rest, due to a significantly larger request payload. There a number of dependencies required to get Angular 2 running in current browsers, which really skyrocket end file size.   

**Compiled JS:** 1.7MB<br>
**Source Code:** [https://github.com/ahoef/angular2-flashcard-app](https://github.com/ahoef/angular2-flashcard-app)

<br>	

###In a nutshell###

Ultimately I liked making the Angular 2 flashcard app the best! I found that out of the three, it was the quickest to get up and running, and the most enjoyable to build. But I should note that I do have a fairly extensive background in Angular 1, so I wasn't learning the framework from scratch with this app. And although it was fun to work with, Angular 2 is still in beta, so adopting it for high-profile production sites right now could be hairy, especially because of the required polyfills.

As for React, I still have much more to learn, and I look forward to gaining more expertise with it. I'm gradually becoming more familiar with Redux, as it was chosen as a key player in this project at work. Only a few weeks into development, my team has found a number of takeaways. For example, we learned that out-of-the-box Redux does not handle async functions all that well, so we've implemented [sagas](https://github.com/yelouafi/redux-saga) for a generator-based approach. We've also learned that the state object can grow very quickly, so it's important to limit top-level properties early on.  

And while I really like using ES2015 syntax and features in my code, I can't see myself ever opting to build a large-scale MVC framework with vanilla JS, as the view layer can get very unweildy very fast. 

That's all for now... next up is a flashcard app in [Elm](http://elm-lang.org/)! Stay tuned! 




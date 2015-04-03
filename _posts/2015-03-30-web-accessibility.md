---
layout: post
title:  "Doing a Better Job at Web Accessibility"
date:   2015-03-30
categories: Web Accessibility
excerpt:  In the back of my mind, I've always known that the websites I've done for fun and for work probably have had some range of accessibility issues. Last night, I took a class on the topic and learned how to run an accessibility audit on sites. The results were... scary! But thankfully, not so hard to fix.
image: "/images/voiceover.png"
---

<img src="/images/voiceover.png">

If you're at all like me, this is what your experience is like when you first start learning about web accessibility:

<ol>
	<li>Check out your site on a screen reader</li>
	<li>Freak out at how bad it is</li>
	<li>Open 300 tabs and research all the things</li>
	<li>Take some steps to fix it</li>
</ol>

Last night I took a GDI class on the topic, taught by my friend [LeeAnn](http://www.twitter.com/_leekinney), so all of this is fresh on my mind! In the class, LeeAnn provided a wealth of information about accessibility best practices, as well as an extensive list of tools to test and improve code for better accessibility.

### A few key takeaways: ###

<ul>
	<li>Web Accessibility doesn't just mean accommodating blind or deaf users 
		<ul>
			<li>It means making sure everyone can use your site, including users with mental & physical disabilities, and users who can't afford up-to-date hardware</li>
			<li>It encourages you to question the necessity of bells and whistles in your design</li>
		</ul>
		<br>
	</li>

	<li>Experiencing a site via a screen reader and auditing for accessibility should be built into regular maintenance and QA
		<ul>
			<li>The keyboard shortcut Command + F5 on a Mac will start the built-in osx Voice Over tool</li>
			<li>There are many free screen readers available for non-Mac users, such as <a href="http://www.nvaccess.org/download/">this</a></li>
		</ul>
		<br>
	</li>

	<li>Writing semantic markup gets you lots of accessibility, basically for free
		<ul>
			<li>Screen readers have context for each piece of content on the page, and can provide users with a better orientation about hierarchy</li>
		</ul>
		<br>
	</li>

	<li>Accessibility does impact the bottom line
		<ul>
			<li>Obviously, no one will buy anything from a site they can't use</li>
			<li>Some countries (Canada & Australia) require accessibility by law, and some companies have been sued/fined for infringement</li>
		</ul>
	</li>
</ul>

### And a few things you can start using in your code right away: ###

Alt text for images is super important:

{% highlight html %}
<img src="philadelphia-skyline.jpg" alt="Philadelphia skyline at sunset">
{% endhighlight %}

Content that is hidden by the display: none CSS property does not get read by a screen reader. You can visually hide content but include it in screen readings by using this snippet:

{% highlight css %}
.visuallyhidden {
    border: 0;
    clip: rect(0 0 0 0);
    height: 1px;
    margin: -1px;
    overflow: hidden;
    padding: 0;
    position: absolute;
    width: 1px;
}
{% endhighlight %}


Opening links in new tabs is disorienting for screen reader users; include a hidden span to indicate what's happening:

{% highlight html %}
<p>Here's a paragraph with a link to <a href='http://girldevelopit.com' target=
'_blank'>Girl Develop It<span class="visuallyhidden"> that opens in a new 
window</span></a>.</p>
{% endhighlight %}


### And a few really great resources: ###

* [a11Y Project](http://a11yproject.com/)
* [WebAIM](http://webaim.org/)
* [Accessibility Checklist](http://design4access.nomensa.com/checklist.html)
* [WAVE Toolbar Chrome Ext](http://wave.webaim.org/toolbar/)
* [Chrome Color Contrast Analyzer](https://chrome.google.com/webstore/detail/color-contrast-analyzer/dagdlcijhfbmgkjokkjicnnfimlebcll?hl=en)


In short, making accessible websites entails semantic development patterns and thoughtful designs, with neither of which you can really go wrong in the end. Another friend, [Lisa](http://www.twitter.com/_lisli), pointed me to this site when I was on an accessibility roll on Twitter after learning so much last night- it sums things up pretty nicely: [http://motherfuckingwebsite.com/](http://motherfuckingwebsite.com/)




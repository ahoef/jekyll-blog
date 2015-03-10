---
layout: post
title:  "Opening Sublime Text from the Command Line"
date:   2015-03-09
categories: command line, sublime text, text editors, bash profile
excerpt: If you use the command line for commiting files in version control, you probably also find yourself navigating through directories via the command line. Cd'ing into a folder and opening it in Sublime Text with a single short command can be a real handy workflow step &mdash; so here's how to do it! 
---


https://gist.github.com/olivierlacan/1195304

-osx disclaimer

-show hidden files on mac osx to look at bash profile

-seems to be compatible with ST3

-troubleshooting: close terminal and relaunch


<code class="terminal">
$ sublime .
</code>

If you want to spice things up, you can change the last parameter of the symlink above to something exciting, like: 

<code class="terminal">
$ ln -s /Applications/Sublime\ Text\ 2.app/Contents/SharedSupport/bin/subl /usr/local/bin/blastoff
</code>

Which you'd then use like this:

<code class="terminal">
$ blastoff .
</code>







---
layout: post
title:  "Opening Sublime Text from the Command Line"
date:   2015-03-09
categories: command line, sublime text, text editors, bash profile
excerpt: If you use the command line for commiting files in version control, you probably also find yourself navigating through directories via the command line. Cd'ing into a folder and opening it in Sublime Text with a single short command can be a real handy workflow step &mdash; so here's how to do it! 
---

If you use the command line for commiting files in version control, you probably also find yourself navigating through directories via the command line. Cd'ing into a folder and opening it in Sublime Text with a single short command can be a real handy workflow step. If you use a Mac, here are some simple instructions:

Since you'll need to look at your bash profile, the first step is to configure your finder to show hidden files. If you are able to see hidden files, skip to this part!

From the root directory in your terminal, enter

<code class="terminal">
$ defaults write com.apple.finder AppleShowAllFiles YES
</code>

Then, fully quit Finder to affect these changes by entering

<code class="terminal">
$ killall Finder
</code>

Now when you reopen Finder, you should see your hidden files faded in opacity.





https://gist.github.com/olivierlacan/1195304


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

Again these steps are just for Mac users, but [this Scotch.io tutorial](https://scotch.io/tutorials/open-sublime-text-from-the-command-line-using-subl-exe-windows) looks like a good walkthrough for Windows users. 





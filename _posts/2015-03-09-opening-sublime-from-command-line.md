---
layout: post
title:  "Opening Sublime Text from the Command Line"
date:   2015-03-09
categories: command line, sublime text, text editors, bash profile
excerpt: If you use the command line for commiting files in version control, you probably also find yourself navigating through directories via the command line. Cd'ing into a folder and opening it in Sublime Text with a single short command can be a real handy workflow step &mdash; so here's how to do it! 
---

If you use the command line for commiting files in version control, you probably also find yourself navigating through directories via the command line. Cd'ing into a folder and opening it in Sublime Text with a single short command can be a real handy workflow step. If you use a Mac, here are some simple instructions:

Since you'll need to look at your bash profile, the first step is to configure your finder to show hidden files. If you are able to see hidden files, skip this part!

From the root directory in your terminal, enter

<code class="terminal">
$ defaults write com.apple.finder AppleShowAllFiles YES
</code>

Then, fully quit Finder to affect these changes by entering

<code class="terminal">
$ killall Finder
</code>

Now when you reopen Finder, you should see your hidden files faded in opacity.


So to set up this shortcut, there are a few different approaches to take. One approach is to set up an alias in your bash profile, and another is to create a symlink to Sublime's built-in command line tool. Both will get the job done!

Approach #1 &mdash; Bash profile alias

An alias is what it sounds like- it's is nothing more than a custom keyboard shortcut for running a terminal command. Commonly aliases live in a file called .bash_profile, which is a file that houses custom settings for your computer. And commonly, .bash_profile is located at the root directory. Depending on whether you have a brand new computer or if you've inherited one, a .bash_profile may or may not exist.

To check if a .bash_profile exists on your computer, go to your root directory:

<code class="terminal">
$ cd ~
</code>

Then, list all of the files, including hidden ones, because .bash_profile is a hidden dotfile:

<code class="terminal">
$ ls -a
</code>

If you see .bash_profile listed as a file in the directory, pop over to the finder and drag it into Sublime, or open it up in the default terminal text editor.

If you don't see it, you can create it with this command:

<code class="terminal">
$ touch .bash_profile
</code>

So when the file is open in a text editor, we have to write an alias that says 'open this with Sublime'. So paste this in, and just remove the 2 if you are using Sublime Text 3:

```
alias sublime='open -a "Sublime Text 2"'
```

Then, save out your changes to the file, and run this command to see your changes updated in the terminal:

<code class="terminal">
$ source ~/.bash_profile
</code>

Then, cd into the directory you want to open, and run this to open the whole directory in Sublime:

<code class="terminal">
$ sublime .
</code>

If you want to just open a single file, run:

<code class="terminal">
$ sublime index.html
</code>

The cool thing is that you can make your alias name anything you want&mdash; like 'blastoff', for example:

```
alias blastoff='open -a "Sublime Text 2"'
```
<code class="terminal">
$ blastoff index.html
</code>


Approach #2 &mdash; Symlink

A symlink is similar to an alias; it's just a file that contains a reference to another file or folder. [Sublime Text's documentation](http://www.sublimetext.com/docs/2/osx_command_line.html) for using their command line tool for opening files suggests creating a symlink between the tool and a folder called ~/bin/subl. Most Mac users would already have a bin folder located in usr/local, above the root directory, so creating a new bin folder for this purpose isn't necessary.

So, following [Olivier Lacan's Instructions](https://gist.github.com/olivierlacan/1195304), we can just set the symlink to point to the usr/local/bin folder that already exists. At the command line, run the line below. Again, just remove the '\ 2' for Sublime Text 3.

<code class="terminal">
$ ln -s /Applications/Sublime\ Text\ 2.app/Contents/SharedSupport/bin/subl /usr/local/bin/sublime
</code>

Then take a look at your bash profile to see if there's an export PATH that points to your usr/local/bin. If you don't see it, just paste this in, 

```
export PATH=/usr/local/bin:(...)
```

And run this to register the changes you just made in the file:

<code class="terminal">
$ source ~/.bash_profile
</code>

Then to test, run 

<code class="terminal">
$ sublime .
</code>

And like the 'alias' approach above, if you want to spice things up, just change the last parameter of the symlink like so: 

<code class="terminal">
$ ln -s /Applications/Sublime\ Text\ 2.app/Contents/SharedSupport/bin/subl /usr/local/bin/blastoff
</code>

Which you'd then use like this:

<code class="terminal">
$ blastoff .
</code>

Again these steps are just for Mac users, but [this Scotch.io tutorial](https://scotch.io/tutorials/open-sublime-text-from-the-command-line-using-subl-exe-windows) looks like a good walkthrough for Windows users. 





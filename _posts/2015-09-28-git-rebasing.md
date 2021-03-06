---
layout: post
title:  "Git Rebasing"
date:   2015-09-28
categories:
excerpt:  I recently heard something along the lines of how the history of your codebase in version control should not be a list of log entries, but rather a biography of your code. Here's a quick rundown of what rebasing is in git, and how it can help cut out commit cruft so that your repo tells a more compelling story.
image: "/images/rebase-script.png"
---


I recently heard something along the lines of how the history of your codebase in version control should not be a list of log entries, but rather a biography of your code. It's an interesting thought exercise, to reimagine your commit history as a curated narrative. If you like this idea, rebasing can be a useful practice for cleaning up 'the boring bits', like automatic merge commits and other extraneous merge commits, so that your history tells a more compelling story.

Sure, rebasing *is* basically rewriting history, which sounds scary, but when done carefully and correctly, it can really increase the quality and elegance of your code repo.
<br>
<br>

###Automatic Rebasing###
One scenario where you might want to rebase is if changes have been made on master, and you have some other changes on a feature branch, and you want to port over changes from master onto your feature branch so you stay up-to-date.

From your feature branch, if you run `git pull` or `git fetch` and then `git merge` individually, you'll be thrown into Vim or another text editor you have set as default&#42; where you'll be prompted to save a merge commit message. That commit will get its own SHA and live in your history.

See this visual example from the excellent Atlassian article, ['Merging Vs. Rebasing'](https://www.atlassian.com/git/tutorials/merging-vs-rebasing/conceptual-overview)

<img src="/images/git-merge.png" alt="graphic of merge commit">

But if you rebase, you can avoid the merge commit and eliminate forks in history. Here's the order of terminal commands to do an automatic rebase, starting in your feature branch.

Fetch the changes in master:
<code class="terminal">
$ git fetch
</code>

Rebase:
<code class="terminal">
$ git rebase origin/master
</code>

If there are merge conflicts, resolve them in the files.

Add those resolved files to the staging area:
<code class="terminal">
$ git add &lt;filename&gt;
</code>

Confirm you want to continue and finish the rebase:
<code class="terminal">
$ git rebase --continue
</code>

<p class="list-head">So 3 main things happen with that rebase above:</p>
<ul class="automatic-rebase-list">
<li>All changes that are are not in origin/master get moved to a temporary area (the tip)</li>
<li>All origin/master commits are applied one at a time</li>
<li>All commits in temporary area are applied one at a time</li>
</ul>


###Interactive Rebasing###
Another scenario where you might want to rebase is if you have some non-semantic or fragmented commits on your local branch and you want to tidy them up.

If you're thinking of your git history in terms of diary entries vs a narrative, these commits for when you forgot to add one file, or for when you fixed a typo, are like the diary entries for when you lost a sock in the laundry or stepped in a puddle. Not so significant, and not something you really want to be a part of your biography. So you can use interactive rebasing to rewrite your local branch's commit history so that it is succinct and meaningful.

To start an interactive rebase, check your log (`git log`) and think about how far back you want to start doctoring things up. If you want to rebase the last 3 commits, run
<code class="terminal">
$ git rebase -i HEAD~3
</code>

The `-i` flag above just indicates that you want to do an interactive as opposed to automatic rebase, and the `HEAD~3` means that you want to include 3 commits back from your branch's `HEAD`. You could also pass `HEAD^^^` to include the last 3 commits. Or `HEAD~2` for the last 2, or alternatively `HEAD^^`...and so on.

That will open your default text editor&#42; where you'll see a list of your commits, their SHAs, and action keywords, along with some git help text. This is the visual representation of being in that 'temporary area' from above. If you run `git status` back at the terminal, it'll tell you that 'You're not currently on any branch'. Isn't it kind of cool to be floating in this ethereal 'temporary area'?

<img src="/images/rebase-script.png">

<!-- <img src="http://memesvault.com/wp-content/uploads/Philosoraptor-Blank-6.jpg"> -->
<p class="list-head">So in the rebase script in the text editor, you can reword messages, and reorder, split, or combine commits. This is kind of like using a video editor &mdash; you can rewind, edit, and replay your commits the way you like. </p>

<ul>
	<li>
<strong>Reword a message</strong>
<ul>
<li>Find the row containing the commit you want to reword, and then change that action keyword from the default <code>pick</code> to <code>reword</code></li>
<li>Then save and close (<code>:wq</code> in Vim, or <code>Ctrl+0, return, Ctrl+X</code> in nano)</li>
<li>A new editor screen will pop up, prompting you to enter your revised commit message. Just update your text, then save and close&#42;&#42;</li>

<img src="/images/rebase-reword.png"></ul></li>
<li>
<strong>Reorder commits</strong>
<ul>
<li>This is probably the easiest rebase task you can run!</li>
<li>Just copy/paste the rows in the list to the order you want, then save and close</li>
</ul></li>
<li><strong>Split a commit</strong>
<ul>
<li>Change the commit's action keyword from <code>pick</code> to <code>edit</code>, then save and close</li>
<li>You'll be brought back to the terminal prompt, where you can run <code>git reset</code> to wherever your commit was. If it was in the last commit, you'd run <code>git reset HEAD^</code> to unstage the changes in those files</li>
<li>Then add and commit as you normally would in two separate commits</li>
<li>Then run <code>git rebase --continue</code> to seal the deal

<img src="/images/split-commits.png"></li></ul></li>

<li><strong>Combine commits - Approach #1</strong>
<ul>
<li>Change the action keyword to <code>squash</code> for the commit you want to combine with another (one or more commits can be pushed into the previous commit), then save and close</li>
<li>Another editor view will open, where you can delete the second message and edit the first, and save and close

<img src="/images/squash.png"></li></ul></li>

<li><strong>Combine commits - Approach #2</strong>
  <ul>
<li>Just like the <code>squash</code> workflow, you can use a <code>fixup</code> workflow to collapse a commit into the previous commit</li>
<li>Change the action keyword to <code>fixup</code> for the commit you want to combine with another, then save and close</li>
<li>You won't be prompted to edit the commit messages; the combined commits will just retain the message from the first commit. If you decide that you want to change that, just run the <code>reword</code> workflow </li>
</ul></li></ul>

After you've finished your rebase, you can run `git log` to see if your changes came through.

<img src="/images/log.png"/>

And that's about it!

Check out [the docs on git rebase](http://git-scm.com/docs/git-rebase), Code School's [Git Real](https://www.codeschool.com/paths/git) classes, and Atlassian's ['Merging Vs. Rebasing'](https://www.atlassian.com/git/tutorials/merging-vs-rebasing/conceptual-overview) if you're looking for more resources about rebasing.
<br>

&mdash;&mdash;

&#42; To change your default text editor just pass its name in as an arg after the core.editor arg.
<code class="terminal">
$ git config --global core.editor &lt;text editor&gt;
</code>

I actually don't recommend passing in Sublime here, even though I love Sublime and use it every day, because writing out and saving files doesn't really work well. If you don't like the terminal's default editor Vim, I recommend nano, which is another built in Unix text editor that keeps you right in the terminal.

&#42;&#42; In this screenshot, you'll see that I'm rebasing master. This is because this project was a little demo app that I was the only person working on. If you're collaborating with other developers on a project, make sure to only rebase your own branch, never master!

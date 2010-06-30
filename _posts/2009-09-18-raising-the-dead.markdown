--- 
wordpress_id: 84
layout: post
title: Raising the Dead
wordpress_url: http://panpainter.com/?p=84
---
If you run in to the situation where you (or someone you're working with) has deleted files that shouldn't have been deleted (like a stylesheet that gets used in only one place within a very large project, say), you can recover it easily (assuming you're using Git, of course). Simply find when the file was deleted (this is just one way to do it, of course):

    git log -- path/to/file

This method will show all commits for that file - sort of like a log-view of `git blame`

Then, when you have the commit you want, find a commit prior to that (it's easiest if you use a tool like [GitX](http://gitx.frim.nl/) or [GitK](http://panpainter.com/x/j), which comes with git, to view the tree's history), grab that commit's SHA, and run the following command:

    git checkout [SHA] -- path/to/file

Then commit, and you're good to go!

Note that you *can* solve this problem using git rebase, but that'll flatten your tree. This adds a new commit at the head of your tree, and should be less likely to cause any conflicts or weird history issues.

Permalink: [http://panpainter.com/p/8](http://panpainter.com/p/8)

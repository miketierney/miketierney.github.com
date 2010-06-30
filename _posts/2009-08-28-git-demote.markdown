--- 
wordpress_id: 62
layout: post
title: Git Demote
wordpress_url: http://panpainter.com/?p=62
---
I'm a big fan of Trevor Squires' <a href="http://hoth.entp.com/2008/11/10/improving-my-git-workflow">Git Promote script</a>, but one thing that's bothered me about it since I started using it is that it tends to clutter up my `.git/config` file. So, I made a counter-script for it that I'm calling, appropriately, "git demote"

<pre name="code" class="bash">
    #!/bin/sh
    #
    # Counter-script to Travis Squires' git-promote script
    # (http://hoth.entp.com/2008/11/10/improving-my-git-workflow)
    # Removes a 'promoted' branch from the git config file.
    
    curr_branch=$(git symbolic-ref -q HEAD | sed -e 's|^refs/heads/||')
    
    git config --remove-section "branch.${curr_branch}"
</pre>

To install it, simply create a file called `$HOME/bin/git-demote` (assuming you have `$HOME/bin` in your `PATH`, otherwise place it somehwere that does exist in your `PATH`), then run the following from the terminal:

    chmod 755 $HOME/bin/git-demote

Now, when you're done with a branch, you can clean up your `.git/config` file by running:

    git checkout topic-branch
    git demote

And you're good to go.

You can fork the <a href="http://gist.github.com/177306">Gist</a>, if you don't want to type all that.

Shortlink: <a href="http://panpainter.com/p/7">http://panpainter.com/p/7</a>

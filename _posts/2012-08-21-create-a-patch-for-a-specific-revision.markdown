---
layout: post
title: Create a patch from a specific git revision
excerpt: A quick tip on how to create a git patch from a known revision.
---

## {{ page.title }}

I'm posting this mostly so I don't forget how to do it in the future, but if
you're in a situation where you need to create a patch from a far-past commit
(moving something out of one repository in to another, for example), it's not
exactly obvious. I was faced with this exact dilemma recently, as I was
creating a repo that is designed to extend the
[snipmate-snippets](https://github.com/honza/snipmate-snippets) plugin (and
repository) I use for vim with some custom snippets that I and some of my
co-workers have written.

I knew the revision of the commit that I wanted to create the patch from, and
I knew it was only the one commit. The other restriction I had was that
I didn't want to have more than *one* patch (my repo had a merge commit in it
that brought something like 200 commits in the repository, which would be a lot
of patches to wade through).

So, to create that patch, here's what I did (for the example, the shortened
hash of my revision is a1b2c3d):

<pre name="code" class="shell">
  <code>
  $ git format-patch a1b2c3d~1..a1b2c3d
  </code>
</pre>

The important piece here is the <code>~1</code> that follows my SHA.
Essentially I'm telling git to create a patch using a range of the commit
I want (<code>a1b2c3d</code>) from the one before it. You can, of course, use
any number after the ~, creating a patch for each commit after the starting
point.

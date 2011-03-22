--- 
wordpress_id: 137
layout: post
title: Keeping your SASS Organized
excerpt: On why the directory that holds your source files doesn't need to be anywhere near your generated files.
wordpress_url: http://panpainter.com/?p=137
---

## {{ page.title }}

I've recently started working full-time on a <a href="http://sass-lang.com">SASS</a> (more precisely, <a href="http://sass-lang.com/docs/yardoc/file.SASS_REFERENCE.html">SCSS</a>) driven front-end for the product that I'm helping to build at my day job. Ignoring the really fine-grained details, this is a fairly complicated UI, and I'm trying to maintain a fairly modular approach (one might say <a href="http://oocss.org/">Object Oriented</a>) to my stylesheets, creating re-usable components.

### The Parts Serve the Whole

One of the things I'm leveraging is SASS's inherent ability to pull all of your component files in to one big, happily generated CSS file. So I have on SCSS file for forms, one for the messaging system, one for my global typographic rules, and so on. These all get pulled in to one file, which is great, but (as expected), there are quite a few files to manage. This becomes even more monstrous when you start using directories to group common elements (certain all-encompassing parts of the application have SCSS files just for it, and it makes sense to group them together).

### Enter: The Confusion

I decided that I wanted to keep my style-producing files all in one location, so that it's something of a one-stop-shop when it comes time to do editing. Since I'm working on a Rails application, I utilized the app_name/public/stylesheets/ directory for this purpose. My directory structure looked something like:

<pre name="code" class="shell">
  <code>
    public/
    |
    |-- stylesheets/
          |
          |-- base.css
          |-- forms.css
          |-- grid.css
          |-- text.css
          `-- sass/
                |-- base.scss
                |-- forms.scss
                |-- grid.scss
                `-- text.scss
  </code>
</pre>

Add a few more directories in, a few more top-level files, and you can see that **very** quickly that crucial sass directory gets lost (especially since its name starts with a 's'). It gets even worse when you try to handle SCM ignore rules, since the logical thing to do would be to ignore the whole of the stylesheets directory, but you want the sass directory to be tracked. Doable, but complicated and easily messed up (especially if you're using an *ahem* older system.

### Self Reflection

This was brought to my attention when I first introduced another front-end dev to the new system that I had implemented. The directory structure made perfect sense to me - I was generating files to the parent directory that my source was living in. But, when I stepped back to look at it critically, I realized that it is less than ideal: the source files and the generated files where being given the same sort of hierarchical priority. Anyone who wasn't already familiar with the structure (or who didn't have it explained to them) wouldn't know where to find the correct files to edit.

Worse, given that SASS *generates* the files, rather than interpreting them on the fly, it's conceivable that an uninformed developer would mistakenly modify the generated files, see that the changes were made, and then think that they were good (or be instantly baffled when they try to check in their changes, and find that their working directory is "clean").

### What To Do

So, confronted with these problems, I decided that I needed to find a better solution to this storage problem. I could have moved the location for the generated files inside of the source directory, but that didn't seem to really solve the problem.

I could move the sass directory further up in the public directory, so that it lived side-by-side with the stylesheets directory. However, this is a bit antithetical to the spirit of this particular directory which, in a Rails application, is intended for public-consumable content. I don't necessarily want anyone trying to pull down my SCSS files, since that's not their intended use.

What I decided to do was to place the sass directory inside of the app_name/app/ directory. This way, the SASS files live side-by-side with the views (and the models and controllers), which are processed by the server, and rendered as HTML to the user. This especially makes sense if you use HAML to generate your HTML, rather than Rails' built-in ERB interpreter.

### YMMV

Your own use case might (and probably does) differ from my own, and the important point that I want to make here is that while it might seem convenient at first to have all those files together (since they conceptually serve a similar purpose), it's not necessary, or even necessarily the best way to do it.

### A Quick Technical Note

This is for anyone wanting to do something similar, and is using Rails (if neither of these are true, you can skip to the end):

So, to get this working as expected (since SASS makes some assumptions on where the various files live/will be generated), you'll want to modify your configuration like so:

<pre name="code" class="ruby">
  <code>
    # rename 'sass' to whatever the directory that
    # your sass/scss files live in is called
    Sass::Plugin.options[:template_location] = './app/sass'
    
    # if you want your *.css files to live anywhere other than
    # in public/stylesheets you'll want to change this, too.
    # If not, then you don't need this
    Sass::Plugin.options[:css_location] = './public/stylesheets/generated_files'
  </code>
</pre>

### That's It

I hope this helps save someone else some time and prevents confusion.

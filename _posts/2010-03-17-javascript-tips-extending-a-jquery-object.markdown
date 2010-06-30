--- 
wordpress_id: 130
layout: post
title: "Javascript Tips: Extending a jQuery Object"
wordpress_url: http://panpainter.com/?p=130
---

## {{ page.title }}

Ever find a jQuery plugin that does *almost* everything you need it to do, but you don't want to hack the plugin itself (thus throwing yourself in to [dependency hell](http://en.wikipedia.org/wiki/Dependency_hell))? Well, good news! With jQuery, you have a utility to easily add functionality to an existing object without overwriting the existing code (also known as [monkey patching](http://en.wikipedia.org/wiki/Monkey_patch)).

Note that this can be a bit risky, even though it won't override methods within the original object. Consider yourself warned.

### A real life example

The most practical example of this can be demonstrated with a plugin. I was recently using the [boxy](http://onehackoranother.com/projects/jquery/boxy/) plugin for a project. However, of all the functions that it has, it lacks the ability to call something like `Boxy.close();` and have it unload the object from the DOM. This seems like something that should be present in a modal dialog framework (and indeed, it does include a `.hide()` function, but you have to specify a DOM object inside of a Boxy instance for it to target, which is more cumbersome than I felt it should be). So, I decided I would add one:

    // in a file called 'jquery.boxy.mod.js'
    close: function(target) {
      target = target || '.boxy-inner'; // The boxy-inner class is a safe default target, because it's in boxy objects when the script constructs them
      Boxy.get(jQuery(target)).hideAndUnload();
    }

Now I can call `Boxy.close()` anywhere in my code (in my case, I'm using it in [RJS files](http://api.rubyonrails.org/classes/ActionView/Helpers/PrototypeHelper/JavaScriptGenerator/GeneratorMethods.html), but you could call it in an `onclick` or from an observer if you wanted). For added resilience, I set up the function so that it can pull in an optional 'target' object; Boxy enables the ability to create instances from inline code, so the default classes might not be present, and I wanted to be able to close those Boxy instances too.

### So how does it work?
To set this up properly, you'll want to make sure that you have your base files &ndash; in this case, jquery and jquery.boxy &ndash; called first in your HTML:

    <html>
      <head>
        <!-- insert normal HTML header-y goodness here -->
        <!-- jQuery, minified, from the Google AJAX Libraries API: http://code.google.com/apis/ajaxlibs/documentation/#jquery -->
        <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.4.2/jquery.min.js"></script> 
        <!-- Boxy, from the origin Github repo -->
        <script type="text/javascript" src="http://github.com/jaz303/boxy/raw/master/src/javascripts/jquery.boxy.js"></script>
        <!-- Boxy modifications -->
        <script type="text/javascript" src="scripts/jquery.boxy.mod.js"></script>
        <!-- ... -->

Then in the `scripts/jquery.boxy.mod.js` file:

    jQuery.fn.extend(Boxy, {
      close: function(target) {
        target = target || '.boxy-inner';
        Boxy.get(jQuery(target)).hideAndUnload();
      }
    });

The key here is the `jQuery.fn.extend()` function &ndash; this adds methods to an existing jQuery object. You can [read more about it](http://docs.jquery.com/Core/jQuery.fn.extend) on the [jQuery documentation site](http://docs.jquery.com).

And that's all there is to it! Simple, right?

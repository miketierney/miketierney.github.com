--- 
wordpress_id: 102
layout: post
title: nginx + PHP on Mac OS X
wordpress_url: http://panpainter.com/?p=102
---

## {{ page.title }}

I&rsquo;m a big fan of the nginx server (it&rsquo;s lighter weight than Apache, and at this point I&rsquo;m just more familiar with it, so it&rsquo;s my server of choice). When trying to get it to play nicely with PHP, however &hellip; well, it leaves something to be desired.

But, with some persistence (and [quite](http://henrik.nyh.se/2008/02/php-in-nginx-on-os-x) a [bit](http://kovyrin.net/2006/05/30/nginx-php-fastcgi-howto/) of [help](http://sunblu.sh/2008/04/installing-nginx-and-php-with-fastcgi-on-mac-os-x-105-leopard)), I got it up and running on my development machine. The one thing that I wasn't ever able to find an answer for (at least, an answer that worked) was to have my system start up FastCGI on launch. I really dislike having to start these things by hand and feel that any performance trade-off (always having this run in the background) is well worth it for the productivity boost.

[This article](http://sunblu.sh/2008/04/installing-nginx-and-php-with-fastcgi-on-mac-os-x-105-leopard) addresses using a plist that runs on load (utilizing `launchd`), but as the author notes, it works inconsistently. After doing some digging of my own (by reading the nginx plist that ships with my MacPorts install of the server), I came up with the following:

<pre class="code" name="xml">
  <code>
#### com.panpainter.php-fastcgi.plist
    &lt;?xml version="1.0" encoding="UTF-8"?&gt;
    &lt;!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN"
    "http://www.apple.com/DTDs/PropertyList-1.0.dtd"&gt;
    &lt;plist version="1.0"&gt;
    &lt;dict&gt;
      &lt;key&gt;Label&lt;/key&gt;
      &lt;string&gt;com.panpainter.phpfcgi&lt;/string&gt;
      &lt;key&gt;EnvironmentVariables&lt;/key&gt;
      &lt;dict&gt;
        &lt;key&gt;PHP_FCGI_CHILDREN&lt;/key&gt;
        &lt;string&gt;2&lt;/string&gt;
        &lt;key&gt;PHP_FCGI_MAX_REQUESTS&lt;/key&gt;
        &lt;string&gt;1000&lt;/string&gt;
      &lt;/dict&gt;
      &lt;key&gt;ProgramArguments&lt;/key&gt;
      &lt;array&gt;
        &lt;string&gt;/opt/local/bin/daemondo&lt;/string&gt;
        &lt;string&gt;--label=php-cgi&lt;/string&gt;
        &lt;string&gt;--start-cmd&lt;/string&gt;
        &lt;string&gt;/opt/local/bin/php-cgi&lt;/string&gt;
        &lt;string&gt;-q&lt;/string&gt;
        &lt;string&gt;-b&lt;/string&gt;
        &lt;string&gt;127.0.0.1:8888&lt;/string&gt;
      &lt;/array&gt;
      &lt;key&gt;OnDemand&lt;/key&gt;&lt;false/&gt;
      &lt;key&gt;Debug&lt;/key&gt;&lt;false/&gt;
      &lt;key&gt;RunAtLoad&lt;/key&gt;&lt;false/&gt;
    &lt;/dict&gt;
    &lt;/plist&gt;
  </code>
</pre>

([Get the code here](http://gist.github.com/247418))

After adding this to the `/Library/LaunchDaemons` directory (or symlinking it in to there, if you&rsquo;d rather it live somewhere else), don't forget to run the following to ensure that it&rsquo;ll run on load:

<pre name="code" name="bash">
  <code>
    $ sudo launchctl load -w /Library/LaunchDaemons/com.panpainter.php-fastcgi.plist
  </code>
</pre>

Now, this probably is not the most elegant solution, and I&rsquo;m relying on the MacPorts-depedent `daemondo`, but it works. If you know of a better way to do this, I&rsquo;d be happy to hear it.


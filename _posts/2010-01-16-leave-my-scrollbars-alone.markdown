--- 
wordpress_id: 112
layout: post
title: Leave My Scrollbars Alone
wordpress_url: http://panpainter.com/?p=112
---
One of my biggest pet peeves (right up there with re-sizing my browser), is when a designer changes the way that the scrollbar looks. I will admit that when I first started playing with CSS, this was one of the things that appealed to me. However, in addition to my own ignorance, web design principles still nascent at that point. It was awesome to be able to adjust the way that Internet Explorer's scrollbars looked: I could adjust the aesthetic of the scrollbar to match the way that my site was designed. It didn't work in that pesky up-start of a browser that the Mozilla folks were producing, but I was ok with that, because the majority of people weren't using that browser (it pains me to think that I actively stuck by Internet Explorer at that point, but there it is. I've learned, and moved on from that horrendous browser).

Now, all of this reminiscing has a point: Tim Van Damme's recent update to [his blog](http://maxvoltar.com). Normally I really admire his work, and he's an exceptional interface designer. Recently, he launched an update to his blog, which, as expected, is fantastic. Except for one thing: he changed the scrollbars.

From an purely aesthetic point of view, it's really well executed. It fits the updated design of the site, and it doesn't vary too greatly from established scrollbar elements (it's very much like the iPhone scrollbar). I do appreciate that it looks appropriate in that context.

However, there are some technical and accessibility issues with it, primarily that you can't use the keyboard to progress down the page. Google has similar issues with their Javascript-driven [scrollbars in Wave](http://ignorethecode.net/blog/2009/11/15/google_waves_scrollbars), though they get around it by enabling conversations to be selected with the keyboard, and the subsequent selections will let you progress down the page. However, with the technique that Tim's using (and the context that it's being used within) causes me the greatest concern because there's really no way to navigate through his site using just the keyboard, which for me is inconvenient, but for others it basically makes his site very unusable.

Furthermore, from a usability standpoint, by doing this, he has fundamentally changed the viewing experience for his site's visitors. No longer can they assume that the scrollbar will behave as expected, because it looks different (also, he's removed the up/down arrows that some people still rely on). If nothing else, it takes the focus away from the content of his (very well written) articles, and places it on the scrollbars.

My biggest fear with this new implementation is that many of the copy-cat designers out there will see what he's done (which is pushing the envelope, and usually a Very Good Thing) and will implement that without thinking about the impact. I think Tim can get away with it (although, again, I find it annoying) because his implementation is well executed, and his target audience is, for the most part, probably not restrained to their keyboard for navigation around the internet. Most importantly, the implementation he's using only targets Webkit-based browsers.

So, while using these newer techniques is a wonderful thing, it's best to proceed with caution when you're doing something that will fundamentally change the user's experience. I don't want to seem like I'm warding off new advanced techniques, just that anyone choosing to use it should be wary of the impact of that implementation.

Short link: <a href="http://panpainter.com/p/11">http://panpainter.com/p/11</a>

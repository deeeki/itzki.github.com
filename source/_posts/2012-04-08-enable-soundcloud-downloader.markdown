---
layout: post
title: "Enable SoundCloud downloader"
date: 2012-04-08 19:09
comments: true
categories:
---

I like [SoundCloud](http://soundcloud.com/).

Recently, downloading mp3 streaming file (of course, unofficial) is not working on Chrome. I use this userscript.
([Soundcloud Super +2: Downloader + Recommender for Greasemonkey](http://userscripts.org/scripts/show/96022))

At first, I couldn't find the cause. But finally, I got it.

Following code is a part of Javascript at SoundCloud.

``` javascript base.js http://a1.sndcdn.com/javascripts/base.js
$(document).bind('DOMNodeInserted', function (e) {
    if (e.srcElement && e.srcElement.outerHTML &&
        e.srcElement.outerHTML.indexOf("soundcloud.com/stream") !== -1) {
        $(e.srcElement).remove();
    }
});
```

That makes us rejection of downloading. So we need to remove the behavior.

In the case of "SoundCloud Super +2", add following code on the userscript.

``` javascript example of "SoundCloud Super +2"
contentEval('$(document).unbind("DOMNodeInserted");'); //add
contentEval(...);
```

Re-install edited userscript, then visit SoundCloud.

![soundcloud](/images/20120408.jpg)

It works!

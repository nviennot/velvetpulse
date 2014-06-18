---
layout: post
title: "A Measurement Study of Google Play"
description: ""
category: 
tags: []
---

Abstract
---------

Although millions of users download and use third-party Android applications
from the Google Play store, little information is known on an aggregated level
about these applications.  We have built PlayDrone, the first scalable Google
Play store crawler, and used it to index and analyze over 1,100,000 applications
in the Google Play store on a daily basis, the largest such index of Android
applications.  PlayDrone leverages various hacking techniques to circumvent
Google's roadblocks for indexing Google Play store content, and makes
proprietary application sources available, including source code for over
880,000 free applications.  We demonstrate the usefulness of PlayDrone in
decompiling and analyzing application content by exploring four previously
unaddressed issues:  the characterization of Google Play application content at
large scale and its evolution over time, library usage in applications and its
impact on application portability, duplicative application content in Google
Play, and the ineffectiveness of OAuth and related service authentication
mechanisms resulting in malicious users being able to easily gain unauthorized
access to user data and resources on Amazon Web Services and Facebook.

The Talk
--------

<iframe width="640" height="360" src="http://www.youtube.com/embed/xS0lyL_0OAM" frameborder="0" allowfullscreen="1"> </iframe>

Slides are available [here](/assets/playdrone-slides.pdf).

The Paper
---------

Our study appears in the
[Sigmetrics'14](http://www.sigmetrics.org/sigmetrics2014/) conference and
received the outstanding student paper award.

You can download the paper [here](/assets/playdrone.pdf).

Press Release
-------------

* [Columbia Engineering Stars at Sigmetrics 2014](http://engineering.columbia.edu/columbia-engineering-stars-sigmetrics-2014)
* [Columbia Engineering Team Finds Thousands of Secret Keys in Android Apps](http://engineering.columbia.edu/columbia-engineering-team-finds-thousands-secret-keys-android-apps-0)

The Sources
-----------

The sources are located at <a href="https://github.com/nviennot/playdrone">https://github.com/nviennot/playdrone</a>. Have fun with it :)

Nico.

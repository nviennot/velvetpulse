---
layout: post
title: "Scribe: The Deterministic Transparent Record/Replay Engine"
description: ""
category: 
tags: []
---
{% include JB/setup %}

What is Record/Replay
---------------------

Deterministic application record and replay is the ability to record
application execution and deterministically replay it at a later time.
Record-replay has many potential uses, including diagnosing and debugging
applications by capturing and reproducing hard to find bugs, dynamic
application analysis by performing costly instrumentation on replicas that
replay application behavior recorded on production systems, intrusion analysis
by capturing intrusions involving non-deterministic effects, and
fault-tolerance by providing replicas that replay execution and at the
occurrence of a fault, go live in place of the previously running application
instance.

**TL;DR** Scribe is like a time machine for application executions.

Scribe Demo
-----------

<div class="screencast" markdown="1">
<video class="video-js vjs-default-skin" controls="controls" poster="/assets/themes/the-minimum/img/screencast_poster_scribe.jpg"
    width="690" height="388" preload="true" data-setup="{}">
  <source type="video/mp4" src="http://velvetpulse.s3.amazonaws.com/screencasts/scribe.mp4" />
</video>
[download mp4](http://velvetpulse.s3.amazonaws.com/screencasts/scribe.mp4) (12:43 / 720p / 37Mb)
</div>

I Need Help
-----------

Scribe is definitely not production ready. In fact it's not just unstable, it's
also incomplete.  I need hackers to join the project, I cannot
do it by myself: I've dedicated three years of my life on this project, and
I've had enough of being the only one on the code, because I know that this
is much bigger than me.

[The code](https://github.com/nviennot/linux-2.6-scribe/tree/master/scribe)
quality is fairly high, but it needs a lot of improvements.

This is how I see the roadmap:

* **Stability**: Lots of application don't work. The ones that used to
  work on the original prototype like Firefox and OpenOffice don't anymore for
  obscure reasons that I cannot really explain. <rant>I've been chasing a futex
  bug for a year.  Please someone find it so that I can find peace. Okay... I'm
  being a bit dramatic.</rant>

* **Interpreters**: Scribe is transparent, meaning it requires no changes to
  the applications. In some cases, it actually make sense to change the application.
  For example, the internal of the Ruby/Python/Java VM don't need to be recorded.
  I started to patch the Ruby interpreter to make it Scribe aware
  (see [here](https://github.com/nviennot/rubyscribe)). Mutable replay works
  much better when it has context.

* **Distributed**: I want to be able to record an application that spawns
  on multiple servers. Because Scribe records all the interactions the application
  has with its external environment, you don't want to record separately
  the database and the application.

I am also a web developer. My dream is to have the entire stack recorded. When
a user clicks on the "Feedback" button, I want to be able to replay the whole
system on my development machine and see exactly what the user got, replaying
her entire session. I want to replay it faithfully down to the race that
may have happened in the database. I also want to be able to modify the code to
understand it better while it's replaying. With enough brains on this, we can
make it a reality.

Try it
-------

You can download an Ubuntu 12.04 VM loaded with vanilla Scribe
[here](http://velvetpulse.s3.amazonaws.com/vm/scribe-ubuntu-1204.tar.bz2) (1.1Gb).  
You need [VMware Workstation](http://www.vmware.com/products/workstation/overview.html) or
[VMware Player](http://www.vmware.com/products/player/overview.html) (free) to use it.  
Login with root/root.

You will need to update Scribe you are in because it's a bit out of date.

Scribe is a Framework
---------------------

In the video, I spend some time showing what we call *mutable replay*. It's a
new concept that we've introduced and formalized. I will be presenting our
research next March at the ASPLOS conference.

The mutable replay engine plugs into the Scribe engine through the Python
library. The mutable replay sources are not yet distributed.

We have built other projects around the Scribe engine such as Racepro,
a process race detection engine.

Scribe gives you an API to record/replay applications, and fiddle with their
state.

Scribe Publications
-------------------

These papers are a must read for anybody who wants to understand more:

* Transparent Mutable Replay for Multicore Debugging and Patch Validation  
  Nicolas Viennot, Siddharth Nair, and Jason Nieh,
  *Eighteenth International Conference on Architectural Support for Programming Languages and Operating Systems (ASPLOS 2013),
  Houston, Texas, March 2013*. To appear.

* [Pervasive Detection of Process Races in Deployed Systems](http://viennot.biz/sosp2011_racepro.pdf)  
  Oren Laadan, Nicolas Viennot, Chia-Che Tsai, Chris Blinn, Junfeng Yang, and Jason Nieh,
  *Proceedings of the Twenty-third ACM Symposium on Operating Systems Principles (SOSP 2011), Cascais, Portugal, October 2011*

* [Transparent, Lightweight Application Execution Replay on Commodity
  Multiprocessor Operating Systems](http://viennot.biz/sigmetrics2010_scribe.pdf)  
  Oren Laadan, Nicolas Viennot, Jason Nieh,
  *Proceedings of ACM SIGMETRICS 2010 Conference on Measurement and Modeling of Computer Systems, New York, NY, June 2010*

Sources
-------

* [The Kernel](https://github.com/nviennot/nviennot/linux-2.6-scribe)
* [The Userspace C Library](https://github.com/nviennot/nviennot/libscribe)
* [The Python Library](https://github.com/nviennot/nviennot/py-scribe)

Installation instructions are in the kernel repository.

Acknowledgments
---------------

* My friend [Sid Nair](http://sid-nair.com) implemented the mutable replay engine with me, he was great
  to work with.  
  Without Sid, mutable replay it would still be fiction.
* My PhD mentor [Oren Laadan](http://www.cs.columbia.edu/~orenl/) heavily brainstormed with me during
  the implementation of the first scribe prototype. He had all the good ideas.
* My PhD advisor [Jason Nieh](http://nieh.net/) asked the right questions at the
  right moment, and pushed me really hard to make it happen.

Thank you.

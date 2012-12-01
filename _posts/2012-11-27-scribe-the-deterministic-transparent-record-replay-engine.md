---
layout: post
title: "Scribe: The Deterministic Transparent Record/Replay Engine"
description: ""
category: 
tags: []
edited: November 30 2012
---
{% include JB/setup %}


### <span>Table of Content</span>

» [What is Deterministic Record/Replay](#what_is_deterministic_recordreplay)  
» [What is Scribe](#what_is_scribe)  
» [Watch Scribe in Action](#watch_scribe_in_action)  
» [Scribe is Extensible](#scribe_is_extensible)  
» [Scribe is Work-in-Progress](#scribe_is_workinprogress)  
» [Scribe is Open-source](#scribe_is_opensource)  
» [Scribe Publications](#scribe_publications)  
» [Acknowledgments](#acknowledgments)  

What is Deterministic Record/Replay
-----------------------------------

Deterministic application record and replay is the *ability to record
application execution and deterministically replay it at a later time*.
Record-replay has many potential uses:
* Diagnosing and debugging applications by capturing and reproducing hard to find bugs.
* Dynamic application analysis by performing costly instrumentation on replicas that
replay application behavior recorded on production systems.
* Intrusion analysis by capturing intrusions involving non-deterministic effects.
* Fault-tolerance by providing replicas that replay execution and at the occurrence of
a fault, go live in place of the previously running application instance.

What is Scribe
--------------

Scribe is a record/replay engine to provide deterministic execution
record and replay of generic applications on Linux.

* **Deterministic**: replay produces the same outcome (filesystem, network, etc) recorded.
* **Transparent**: no change, relink, or recompile applications/libraries, no specialized hardware.
* **Multi-process/threaded**: record and replay of applications with multiple processes and threads.
* **Go-Live**: replay of a recorded execution can transition to live execution at any point.
* **Low runtime overhead**: efficient record and replay of both server and desktop applications.
* **Robust**: tested on a wide variety of applications, such as Apache, MySQL, Nginx, MPlayer, [and more](#scribe_publications).
* **Extensible**: python API to control record and replay behavior and to explore execution recordings.

**TL;DR** Scribe is a time machine for applications -- some wizardry to bend the time-space continuum.

Watch Scribe in Action
----------------------

<div class="screencast" markdown="1">
<video class="video-js vjs-default-skin" controls="controls" poster="/assets/themes/the-minimum/img/screencast_poster_scribe.jpg"
    width="690" height="388" preload="true" data-setup="{}">
  <source type="video/mp4" src="http://velvetpulse.s3.amazonaws.com/screencasts/scribe.mp4" />
</video>
[download mp4](http://velvetpulse.s3.amazonaws.com/screencasts/scribe.mp4) (12:43 / 720p / 37Mb)
</div>

Scribe is Extensible
--------------------

Scribe records application execution into a log file, and can later replay
the same application from the log file. It has APIs to inspect the log files,
to modify the logged execution, to control the recording and replaying and
fiddle with its state.

### <span>But there is more to Scribe ...</span>

* Scribe can do tandem-like application execution, where the recording on one
host is streamed to a second host and replayed in real time.
* Scribe can be used to record application execution, then modify the
resulting log to force difference behavior when the application is replayed.
For eaxmple, replay an multi-process applications with different scheduling to
automatically expose and detect harmful race conditions
([Racepro](#scribe_publications)).
* Scribe can be used to record application exeuction, then replay a slight
modified application while tolerating certain divergence from the expected
execution indicated in the logs. For example, replay an application with
debugging enabled from a recording without debugging output. ([mutable
replay](#scribe_publications)).

Indeed, in the video, I spend some time showing what we call *mutable replay*.
It's a new concept that we've introduced and formalized. I will be presenting
our research next March at the [ASPLOS 2013](http://asplos13.rice.edu/) conference.
The mutable replay engine plugs into the Scribe engine through the Python
library. The mutable replay sources are not yet distributed.


Scribe is Work-in-Progress
--------------------------

Scribe is pretty robust, but not production ready. Some applications are not
well supported. I am looking for hackers to join the project, as I cannot
do it by myself: after having dedicated three years to this project, it has
reached the complexity level that evidently demands additional brain power.

[The code](https://github.com/nviennot/linux-2.6-scribe/tree/master/scribe)
quality is fairly high, but there is room for improvements.

This is how I envision the roadmap:

* **Coverage**: Some applications don't replay well. Some used to work on the 
  original prototype, like Firefox and OpenOffice, but not anymore for obscure
  reasons that need further exploration.

* **Interpreters**: Scribe is transparent, meaning it requires no changes to
  the applications. In some cases, it actually does make sense to change the application.
  For example, if the goal is to record and replay programs in languages like 
  Ruby/Python/Java, we may get away without record an replay of the internals
  of the respective VM. In fact, I started to patch the Ruby interpreter to make
  it Scribe aware (see [here](https://github.com/nviennot/rubyscribe)). Mutable
  replay works much better when it has context.

* **Distributed**: I want to be able to record an application that spans multiple
  servers. Because Scribe records all the interactions the application
  has with its external environment, you don't want to record separately
  the database and the application.

### <span>“I have a Dream”</span>

With these three componants in place, I can fullfil a dream: being a web developer, I'd
like to have an entire web stack recorded. When a user clicks on the "Feedback" button, I would replay the whole 
system locally and observe exactly what the user got by replaying her entire session. With that, I'd like to replay it faithfully down to the race that
may have happened in the database. I'd also like to be able to modify the code to
understand it better *while it's replaying*. With enough brains on this, we can
make it a reality. I will expand on this idea in another blog post.

<hr class="fancy" />

Scribe is Open-source
---------------------

### <span>Git Repositories</span>

* [The Kernel](https://github.com/nviennot/linux-2.6-scribe)
* [The Userspace C Library](https://github.com/nviennot/libscribe)
* [The Python Library](https://github.com/nviennot/py-scribe)

Installation instructions are in the kernel repository.

### <span>Try it yourself</span>

Download an Ubuntu 12.04 VM loaded with vanilla Scribe
[here](http://velvetpulse.s3.amazonaws.com/vm/scribe-ubuntu-1204.tar.bz2) (1.1Gb).  
You need [VMware Workstation](http://www.vmware.com/products/workstation/overview.html) or
[VMware Player](http://www.vmware.com/products/player/overview.html) (free) to use it.  
Login with root/root.

Note: the VM does not contain the latest version Scribe, so you may want 
to update the sources and recompile before you start to play with it.

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

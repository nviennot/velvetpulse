---
layout: post
title: "tmate open-sourced: Instantly share your terminal through your private servers"
description: ""
category: 
tags: []
edited: October 5 2013
---



<div style="margin: 0 auto; width: 660px;">
<img alt="tmate" src="/assets/img/tmate-arch.svg" style="display: inline; width: 300px" />
<img alt="tmate" src="/assets/img/tmate.png" style="display: inline; width: 350px"  />
</div>

---

tmate is a tool to instantly share your terminal. Learn more on [http://tmate.io/](http://tmate.io).  
Due to popular demand, the server side is finally open source!  
No more trust issues :)

Host your own tmate server
--------------------------

The sources are located at <a href="https://github.com/nviennot/tmate-slave">https://github.com/nviennot/tmate-slave</a>.

You need a Linux distribution as tmate utilizes linux specific features.  
Here is how you can run your own tmate server.


    git clone https://github.com/nviennot/tmate-slave.git && cd tmate-slave
    ./create_keys.sh # This will generate SSH keys, remember the keys fingerprints.
    vim tmate.h # Edit TMATE_DOMAIN with yours.
    ./autogen.sh && ./configure && make
    ./tmate-slave

Community
----------

The tmate sources are released under the MIT license, so feel free to do
whatever you want with it.  
Feel free to to integrate tmate with your service, something like
[Showterm](http://showterm.io/), [Airpair.com](http://www.airpair.com/),
[MadEye](https://madeye.io/), or [Floobits](https://floobits.com/).

I would really like a platform where people can share their terminal
publicly when working on open source projects. I'd like to see experts
working on their projects to learn new workflows. And perhaps jump in
to pair with a stanger :)

For more information on tmate check out [http://tmate.io/](http://tmate.io).

Have a nice day,  
Nico.

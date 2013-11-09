---
title: Keeping Time on Frequently-suspended Virtual Machines
author: louis
layout: post
permalink: /2012/02/02/keeping-time-on-frequently-suspended-virtual-machines/
categories:
  - Linux
  - Web Development
---
When everyone else&#8217;s git commits start showing up as &#8220;in the future,&#8221; you know something&#8217;s wrong. 

It&#8217;s probably that your VM, having been suspended while your host OS was sleeping, has lost track of what time it is. You *could* `/etc/init.d/networking restart` every time you start work in the morning, but that&#8217;s a drag, and you&#8217;re likely to forget.

Instead, you want to use the NTP daemon to keep your clock in sync. By default, however, NTP will attempt to gradually and incrementally bring your clock back into sync, and will balk at large differences in time (like the 14 hours since you last opened your laptop). Fortunately, there&#8217;s a config setting for that.

Let&#8217;s do this thing:

    sudo apt-get install ntp

Then replace your `/etc/ntp.conf` file with the following:

    tinker panic 0
    
    restrict 127.0.0.1
    restrict default kod nomodify notrap
    
    server 0.au.pool.ntp.org
    server 1.au.pool.ntp.org
    server 2.au.pool.ntp.org
    server 3.au.pool.ntp.org
    
    driftfile /var/lib/ntp/ntp.drift
    

Replace those `server` lines with NTP servers of your choosing.

The important line there is `tinker panic 0`, which is what tells NTP not to freak out if the difference in time between the local clock and the server is larger than it would normally be comfortable with. This line **needs** to come **before** all other config directives.

With that config in place, fire off a quick 
    sudo /etc/init.d/ntp restart

and you&#8217;re in business. Next time you bring your VM back up from a deep slumber, hit `date` and you&#8217;ll be pleasantly surprised.
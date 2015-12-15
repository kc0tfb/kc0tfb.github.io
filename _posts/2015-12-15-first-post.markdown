---
published: true
title: Threat Indicators
layout: post
---
I'm trying out tinypress as a quick, no-nonsense blog platform.  My goal is to jot down notes, thoughts and ideas either for my own use or for sharing with friends and colleagues.  Hopefully this will prove effective enough that I don't need to bother with hosting my own content elsewhere.

Now, onto the topic at hand:  DNS as a threat indicator...

I was consulting for a company that wanted to install Data Loss Prevention software, but they had a few other challenges to overcome as well.  One of which: They had random complaints of network slowness. While waiting for the VM team to find disk resources to fit the project requirements, I decided to sniff some traffic from the network and see if I could help the network team spot anything that might be amiss. I saw a lot more DNS traffic than anticipated for a network this size.  It turned out that an APT had been using DNS (along with other protocols) to tunnel traffic back out - and it had been there for several months before I discovered it. Worse, there was no easy way to turn it off.

The internal environment was a mess - overlapping DHCP zones, unpredictable machine domain membership, DNS servers that didn't know about each other, or pretended to be authoritative for things they weren't, etc.  After a blanket statement of "Your DNS environment is broken.", I whipped up a quick DNS filter using a linux box and dnsmasq.  Directing all the recursive queries to dnsmasq allowed me to properly cache DNS requests (cutting DNS traffic by over half) and gather statistics to identify other anomalies - as well as blocking the malicious domains used by the APT for hiding traffic.

A "maybe someday" goal is to share the configuration I used, perhaps as a Dockerfile, to build this.

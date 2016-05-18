---
layout: post
title:  "Have You Got 80 Million Filing Cabinets Filled With Text to Transfer?"
date:   2013-04-08 12:00:00
categories: juniper technical_tips
---
Last week, as I worked with a customer to test 40 and 100 Gigabit per second Ethernet connections for their new Data Center core, I started remembering some other deployments of new interfaces I've gotten to work with. I remember when we were turning up some of the first 1 Gigabit per second Ethernet connections at Level 3 Communications back in 1999 and 2000 when it required burning an entire router line card for just one port (What do you mean autonegotation with the same vendor fails but it succeeds when connected to another vendor's gear?). Later we turned up some of the first OC-192 router interfaces, again, only one port per slot. At the time one of my colleagues talked about how even though 10 Gig Ethernet and then 100 Gig Ethernet were the obvious headlines, there was a value to using 40 Gbps Ethernet as an achievable intermediate stop between the two. I would imagine he didn't know how close to the terminal that stop would come until 40 and 100 were released as one standard.

As before I wish I had some way to experience the bandwidth we were turning up. There were only some flashing lights and some new interface names in the router output (I find xle for 40 GbE to be clever, not so much et for 100 GbE* in Junos) to alert me that some old hurdle had been crossed. In the past that bandwidth had to last for at least a little while till the promise of new functionality came true. Now, we could rely on Aggregated Ethernet so that our 100 GbE link was almost instantly turned into a 200 Gbps ae0 making activating a 100 GbE link rather forgettable in the grand scheme of things.

I don't really have a point with this post. It's just the kind of thing that I find interesting to note. I look forward to the coming of 1 Terabit per second Ethernet in the future and again I'll probably spare a second to think about the significance of the change in an industry that is always obsoleting it's great achievements of the recent past. The increase in speed isn't the only thing here, but also the change of venue. In 1999 and 2000 when we were turning up those connections at Level 3, we were working to become a vital piece of the global Internet backbone. My customer last week, while a significant company within their own industry, has no aspirations to do anything on the Internet other than facilitate their normal business which has nothing to do with bandwidth. Perhaps this change, more than the continuing increase in bandwidth needed and interface speeds, is truly important.

The title of the post comes from a description of what one can do with 100 Gbps from <a href="http://www.oar.net/network/100gbps">the Ohio Academic Resources Network (OARnet)</a>

* et is also used for 40 GbE on the MX just to confuse everybody.

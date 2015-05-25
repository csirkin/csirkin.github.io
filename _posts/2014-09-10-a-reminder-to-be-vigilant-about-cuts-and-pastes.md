---
layout: post
title:  "A Reminder to be Vigilant About Cuts and Pastes"
date:   2014-09-10 12:00:00
categories: Juniper Technical_Tips
---
I was pasting a big section of a router configuration today and even though I wasn't getting any errors, I decided it would be worth comparing what the router was going to load to what I had pasted in. This is just a tiny part of the 700+ lines I uploaded, but it shows the problem.

I pasted:

[code]
                from {
                    source-prefix-list {
                        bgp-neighbors
                        bgp-neighbors-logical-systems
                    }
[/code]

The router got:

[code]
                from {
                    source-prefix-list {
                        bgp-neighbors;
                    }
[/code]

This same situation was repeated 20 or 30 more times in the config. Do you see the problem with my config?*

If you are pasting in stanza configs like I have above, a somewhat efficient way to check this in Junos is to run "show \| compare \| no-more" then cut and paste the output into a text editor. You can then compare the number of lines in the pasted config to what's on the router and go line by line if there's a discrepancy. Or just add a "\| count" and only do the cut and paste if there's a difference.

*The problem with my config is that it was missing the ';' at the end of the bgp-neighbors line. It should be at the end of the bgp-neighbors-logical-systems line as well, but only the first one was responsible for this issue.

It's also worth noting that in the same config I was pasting something that had been given to me by someone else who had used Word to compose and it contained curly quotation marks instead of good old fashioned ". The router didn't like those and I had to do some global search and replaces in order to make them work. It's much better to use a text editor for composing your configs or at least turn off Word's smart quotation marks auto formatting.

Lastly, if you're pasting large configs you might see some lines disappearing due to flow control problems. I've had good success using this [solution from SecureCRT](http://www.vandyke.com/support/tips/echoflowctrl.html).
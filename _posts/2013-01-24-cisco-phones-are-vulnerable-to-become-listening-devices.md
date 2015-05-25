---
layout: post
title:  "Cisco Phones are Vulnerable to Become Listening Devices"
date:   2013-01-24 12:00:00
categories: reading_the_news
---
There's really two things that I like about [this article](http://arstechnica.com/security/2013/01/hack-turns-the-cisco-phone-on-your-desk-into-a-remote-bugging-device/" title="this" target="_blank"). The first is the title of that talk, "Just because you are paranoid doesn't mean your phone isn't listening to everything you say", and the second is that this is news.

We ought to know by now that our digital lives aren't very secure. In this specific case the problem is that the phones are subject to a physical attack which allows somebody to run malicious code on the phone. That's bad. However, most VoIP phones have some type of interface beyond the handset and they always have a network stack. Through those two interfaces one can attempt to make the phone do something that you -- the user or the administrator -- don't want them to do, whether it's an intended feature or not. Because of that, these phones have administrator passwords. I don't mean a password to keep you from making a call, though many of them have that too, but a password to keep somebody from changing how the phone is configured. 

There have been more than a few times over the past nine or so years that I've been working with these phones, most recently just as a user, and I've needed info that I could only get through the administrator's interface. I've gone to Google; searched for the default password; and I'm in with access to read or change whatever I want. When I first started using Cisco VoIP phones about nine years ago in a pilot deployment that I was working on. I had an hour or two of fun telnetting into people's phones, using the default password and either rebooting the phone, making the lights flash or my personal favorite, having it load up a ship's foghorn ringtone, pumping the volume to full blast and then calling them. 

While a couple of us were figuring out what we could do we realized that we could make the remote phone silently dial us while it was onhook and then turn on the speaker. Playing with our friends a couple cubes down the hall was harmless fun, but we also pointed out to our Security officer that this would enable anybody with access to our campus network to be able to eavesdrop on any phone they wanted.

Mr. Cui and Mr. Solfo have gone a step further by showing that they can make modifications to a phone that they have to break in to but if you don't bother to do the easy things that you can to make yourself a little more secure they won't need to break in. That goes for not setting a password, leaving a default password or using a password that's too common. And if you have a VoIP phone on your desk, you might want to try out that default password.

---
layout: post
title:  "Using Duo to Authenticate Ubuntu"
date:   2016-03-21 00:00:00
categories: ubuntu duo
---
For the past few years I've been using Authy to provide 2 Factor Authentication for my Ubuntu system, but somebody suggested that I take a look at Duo as an alternative. I originally started using Authy because I like the Authy app more than Google Authenticator and because it also offered a few other things it could authenticate. Duo seems to provide more options, including Radius and it offers more for free and I like the push option with a token so that when I try to log in to something instead of going to the app to get a code to type in it pops up a notification asking me whether to approve the log in or not.

Installing shouldn't be very hard. There are installation instructions for integrating with Linux PAM, but I just couldn't get it working on my Trusty Tahir implementation. Then I noticed a note that I had previously ignored.
"Depending on your OS or architecture, the pam_duo.so module might be in /lib64/security instead of the default location /lib/security. If so, specify the full path to pam_duo.so in the PAM config file, such as /lib64/security/pam_duo.so." This is the case with Ubuntu; you'll want to put in the full path and specify /lib64/security.
Once I did that it works like a champ.

---
layout: post
title: "Setting up an f5 virtual lab on OSX with Docker and Ubuntu"
categories: f5 technical_tips Lab
---

1. Purchase a lab license for f5's virtual edition
2. Download and install it in VMWare Fusion
3. Put that it's management onto a custom vmnet which has connectivity to your Mac and external networks (vmnet8 aka Share with my mac should work)
4. Download Ubuntu 16.04 server
5. Follow the instructions for installing Docker on ubuntu at [docker](https://docs.docker.com/engine/installation/linux/ubuntulinux/)
6. I've created a small container image to use for testing, get it with 'docker pull csirkin/alpine-node-echoip'
7. Create a static route on your Mac to reach the containers via the Ubuntu VM's address by 'sudo route -n add 172.17.0.0/16 x.x.x.x'
8. Start the containers, I've been using 3 - 'docker run -i -t csirkin/alpine-node-echoip' then cntl-p cntl-q to get back to your Ubuntu cli. Note the IP address which is reported, though they are most likely 172.17.0.2, 172.17.0.3 and 172.17.0.4.

You now have a relatively lightweight virtual lab with 3 webservers and an f5 virtual appliance. Because the webservers return their IP addresses you can use them to play around with the settings on the f5 for configuring virtual servers and load balancing pools. If you need more functionality from the webservers you can [create your own containers](https://docs.docker.com/linux/step_four/) in place of mine. [Alpine Linux](http://www.alpinelinux.org/) is a great place to start because of its full feature set and small footprint.

Once I can get my hands on it I'm looking forward to modifying this lab with Juniper's containerized SRX.

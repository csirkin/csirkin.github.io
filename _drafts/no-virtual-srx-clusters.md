---
layout: post
title:  "No Virtual SRX Clusters on VMWare Fusion?"
date:   2015-06-01 12:00:00
categories: Juniper Technical_Tips
---
Recently I was working with a customer and looking to test some SRX Cluster configurations that I had written for them before our lab gear became available. No problem I figured, I can easily mock this up with Firefly Perimeter (aka vSRX) on my laptop. However when I tried I couldn't get the links between the members to come up and the boxes never clustered.

Well, there's more than one way to skin a cat I figured, I've got some Junosphere time that I could use to check everything. Wrong again, I had the same problem.

Poking around I found [an article (near the top of page 9)](http://www.juniper.net/techpubs/software/junosphere/junosphere2.0/help/Junosphere_VSRX_Feature_Support_Guide.pdf) which said that SRX clusters weren't supported on Junosphere, but didn't say why. I never did find an article which said that it won't work in VMWare Fusion on my Mac, in fact in [this document](http://www.juniper.net/techpubs/en_US/firefly12.1x46-d10/information-products/pathway-pages/pathway-pages-vmware/security-virtual-perimeter-admin/security-virtual-perimeter-admin-vmware-pwp.pdf) it tells how to set it up in vSphere. But when reviewing the [technical requirements](http://kb.juniper.net/InfoCenter/index?page=content&id=KB25017) of those links -- which, if you'll recall, Juniper says should be direct cable connections rather than passing through a switch if possible -- I found that the Control Plane connections have to have an MTU of 9014 bytes (1632 on the SRX100s). Now when I go back to the [previous document](http://www.juniper.net/techpubs/en_US/firefly12.1x46-d10/information-products/pathway-pages/pathway-pages-vmware/security-virtual-perimeter-admin/security-virtual-perimeter-admin-vmware-pwp.pdf), I find the key line on page 31:

<p>"Click on Properties to turn on the following features:
<br>• General-> Advanced Properties:
<br>• MTU: 9000"

<p>There's only one more piece of the puzzle to find now, and that would be a document saying that you can't set the MTU above 1500 on Fusion interfaces, but I can't find one. And I'm not the only one. While my searches turned up nothing saying it's not supported or telling me how to do it, I found a number of forum questions asking why jumbo frames weren't working.
	
So in the end, it appears that Fusion doesn't support jumbo frames so it isn't able to support vSRX clustering.

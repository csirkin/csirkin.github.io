---
layout: post
title:  "How-to Downgrade Junos on Juniper's QFabric"
date:   2014-08-18 12:00:00
categories: juniper technical_tips
---
Have you searched for how to downgrade a QFabric installation? No? Go ahead and [try it](https://www.google.com/search?btnG=1&pws=0&q=downgrade+qfabric" target="_blank). The first listing looks promising, but it's for the bootloader, not the OS. The 2nd listing has a interesting item in the description on Google - "Note: Downgrading software on a QFabric system is not supported."

It's not all that often that I have to downgrade the software version on networking equipment. This can usually be prevented with solid preparation and effective testing. However, there are times when it's necessary. Today is one such time and I'm sitting in a Data Center watching install status screens go by.

I've been building out a QFabric installation for a customer and in the course of their testing we want to test an upgrade to get a good feel for how long it will take and what kind of downtime, if any, they should tell their customers to expect. We've been implementing the current [JTAC recommended Junos version](http://kb.juniper.net/InfoCenter/index?page=content&id=KB21476" target="_blank") and need to downgrade so that we can reupgrade. Another option would be upgrading to the same version but they felt better upgrading from one version to another.

Most Junos devices will let you upgrade (request system software add) to an older version, but as noted above, not QFabric. In order to downgrade QFabric I’m going to reinstall an older version from USB. There’s [a document on Juniper's site](http://www.juniper.net/techpubs/en_US/junos13.1/topics/task/configuration/qfabric-initial-setup.html" target="_blank") which describes the initial install process. There's [another document](http://www.juniper.net/techpubs/en_US/junos13.1/topics/task/installation/junos-software-reinstallation-preparing-qfx-series-qfabric.html" target="_blank") that describes restoring a corrupted or messed up implementation. I’ve incorporated those and added steps and observations to finish up with a downgraded QFabric installation.

<ol>
<li>Prepare
<ul><li>You’ll need a 4 GB or larger USB flash drive that you can wipe everything off of to burn the QFabric installer to. You might want a 2nd flash drive to backup the configuration, but it’s not necessary.
<li>Backup the Director’s initial configuration by running ‘request system software configuration-backup’ command. This creates a file with the initial setup values that were used for the Director, which include the management IP addresses, etc and also the regular Junos configuration.
<li>Save the current Junos configuration as well with a ‘show configuration’ or any of the other methods of doing this.
<li>Download an older QFabric installer and burn it to the larger flash drive as described in the <a href="http://www.juniper.net/techpubs/en_US/junos13.1/topics/task/installation/junos-software-reinstallation-preparing-qfx-series-qfabric.html" target="_blank">recovery doc</a> mentioned above. I did this with a generic USB drive and performed the steps on my Mac and it worked fine even though it wasn't a Juniper drive and I wasn't on Linux.</ul>

<li>Shutdown the backup Director device. You can do a 'request system halt director-device', but I just pulled the power plugs.

<li>Logon to the primary Director device via its console port and reboot it then start the software install. Once the director boots up (it took about 9.5 minutes to get to this point, if you miss it you’ll have to reboot again) you’ll see the following:
<code>Juniper Networks QFabric Director Install/Recovery Media
-  To boot from the local disk, wait 10 seconds or press the Enter key.
-  To reinstall the QFabric software on this Director device, type: install</code>

Type ‘install’ and then wait. 

<li>Once the installation is complete (the installation on the primary Director took 20-30 minutes) you’ll see the same choices again. Either wait 10 seconds or press enter to boot from the local disk. 

Then restore the configuration to the primary Director.

<code>Before you can access the QFabric system, you must complete the initial setup of the Director group by using the steps that follow.
If the initial setup procedure does not complete successfully, log out of the Director device and then log back in to restart
 this setup menu.

Continue?[y/n]</code>

Press ‘n’ and enter to bypass the script and be asked to restore from backup. Again, the <a href="http://www.juniper.net/techpubs/en_US/junos13.1/topics/task/installation/junos-software-reinstallation-preparing-qfx-series-qfabric.html" target="_blank">recovery doc</a> has instructions on how to mount your USB if you backed up to it. Personally, I found it easier to go to the CLI on the director, vi a new filename in the /var/tmp directory and paste the file contents out of my scroll buffer.

<li>Logon to the powered down backup Director device via its console port and power it on then start the software installation (again, it takes about 9.5 minutes to get to the point where you have to tell it to install and you don’t want to miss it) the same way you did on the primary. Once it reboots it will discover and join the primary Director to form a director group. It takes much longer to finish the install because it needs to form a disk cluster with the primary Director, so expect about an hour.

<li>Login to the Director group VIP and restore the system configuration backup with ‘request system software configuration-restore /path/to/filename’.

<b>NB:</b>The last two steps don’t provide much in the way of feedback. You could log in to a device's console if available and see the progress. Otherwise you can wait about 10 minutes and verify that the devices have completed their boot up and downgrade with ‘show fabric administration inventory’ and make sure that all the devices are present. When checking the fabrics you may want to make the output more specific by using the flag ‘interconnect-devices’ since you may have to wade through all of your node-groups to find them.

<li>Reboot the node-groups with ‘request system reboot node-group all’. As they restart the node devices will attach to the directors and load the current Junos. All the node devices reboot and upgrade at the same time if you do this, which takes about 10 minutes.

<li>Reboot the fabrics with ‘request system reboot fabric’ which will result in the fabrics loading the current Junos as well. Again, all the fabrics reboot at the same time and it took a little less than 10 minutes.

<li>Verify that everything worked by running a ‘show version component all’ and looking to see that each device is running the expected version of Junos.

All told the downgrade took about 3.5 hours. This was going at a very conservative pace and doing lots of checking (more than I have detailed above, feel free to verify much more than I’ve instructed). I didn’t actually power down the backup director until after I was done upgrading the primary director, which probably means that the environment was available until that time, but we didn’t test that.




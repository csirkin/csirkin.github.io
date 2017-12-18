---
layout: post
title: How to upgrade Plex on the Drobo 5n
categories: technical_tips
tags:  plex, drobo
---
This is one of those things that is probably documented very well somewhere but
I couldn't find it so I'm writing up how to do it. I don't take credit (or
  responsibility) for any of the programs mentioned here; I'm just a grateful
  user. This process has worked a few times in a row for me - most recently with version 1.7.5.4035 - so I feel comfortable recommending it.

1. **Make sure that [sudo](http://www.droboports.com/app-repository/sudo/) and
[OpenSSH](http://www.droboports.com/app-repository/openssh/) are installed from
[Droboports](http://www.droboports.com/).** - You'll need to ssh into your drobo
and su to root later and these programs let you do that.
1. **Download the update** - Go to [Plex](https://www.plex.tv/downloads/).
Click the Download button. Use the dropdown menu to select drobo from under
 the NAS category. Then hit the Download button.
1. **Upload the update to the Drobo** - I use scp but you could also drag and drop.
1. **Check the starting version** - ```more /tmp/DroboApps/plex/version.txt```
1. **Unzip plex in a home directory** - You'll want to create a directory and
unzip in there. ```mkdir plex; cd plex; tar zxvf ../plex.tgz```
1. **Su to root** - ```/mnt/DroboFS/Shares/DroboApps/sudo/bin/sudo su -```
1. **Change to the Apps directory** - ```cd /mnt/DroboFS/Shares/DroboApps/```
1. **Stop server** - ```./service stop```
1. **Copy plex to install location** - ```cp -r */path/to*/plex .```
1. **Ignore the update script** - If you run the update script you'll need to
repeat the copy step.
1. **Check the service status** - ```./service status``` If it says that the
service is running then you may have to kill -9 the PID which is
  listed under /tmp/DroboApps/plex/pid.txt or just grab it from running
  ```ps -ef grep plex```.
1. **Start the server** - Once it's no longer running, ```./service start```.
1. **Check the service status** - ```./service status```
1. **Check the version** - ```more /tmp/DroboApps/plex/version.txt``` If it's
running the version that you downloaded then you made a mistake somewhere.
Check the home directory where you unzipped the tar file that the files were
created more recently than the ones in /mnt/DroboFS/Shares/DroboApps/plex. If
they are you can try to copy them again; perhaps your first attempt to copy
didn't work.

Now that I've got that working I need to go try to get Crashplan working again.

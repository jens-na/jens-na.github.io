---
layout: post
title: Mobile PPP connections with ArchLinux' netctl
---

{{ page.title }}
====
Yesterday I bought a SIM card from a communications service provider to
be connected to the internet from everywhere. Luckily my Thinkpad T510
has an integrated WWAN module named [Qualcomm Gobi 2000][3]. 

After the installation
of the firmware you need to configure the connection to your provider.
If you have installed ArchLinux you may want to use the network profile
manager named [netctl][1]. It supports mobile PPP connections out of the box. 

Configuration
====
You have to create a file named `/etc/netctl/mobile` and add the following 
content:

{% highlight bash %}

# Description of the connection, may be anything
Description='mobile connection'

# The device which is attached to your serial usb port
Interface=ttyUSB1

# The connection type
Connection=mobile_ppp

# Username for the connection, provider specific 
User='eplus'

# Password for the connection, provider specific
Password='gprs'

# The access point name (APN), provider specific
# you have to be careful with the quotes
AccessPointName='"internet.eplus.de","0.0.0.0"'

# The PIN code for your sim card, if required
Pin=0000

# Additional mode types, this should be None if you do not have a
# Huawei modem
Mode=None

{% endhighlight %}

Remark to config key 'Interface'
====
As you can see in the example I have entered `ttyUSB1` as my interface.
For the Qalcomm Gobi 2000 modem you can always use this device. If you have
another modem you might need to figure out the device which is connected to
the usb serial port. These commands may help you:

{% highlight bash %}

dmesg | grep ttyUSB
[ 4879.251962] usb 2-1.4: Qualcomm USB modem converter now attached to ttyUSB0
[ 4879.252978] usb 2-1.4: Qualcomm USB modem converter now attached to ttyUSB1
[ 4879.253855] usb 2-1.4: Qualcomm USB modem converter now attached to ttyUSB2
{% endhighlight %}

{% highlight bash %}

ls -1 /sys/bus/usb-serial/devices
ttyUSB0
ttyUSB1
ttyUSB2
{% endhighlight %}

Remark to config key 'AccessPointName', 'User' and 'Password'
====
'AccessPointName', 'User' and 'Password' are provider specific. You need to
figure out these values for yourself. There are lists available on the
WWW.

  - [German provider list][2]


Starting/Stopping procedure
====
If everything is configured correctly you can start the connection with

{% highlight bash %}

netctl start mobile

{% endhighlight %}

If everything worked fine you can see something like this: 
{% highlight bash %}

journalctl -xn
{% endhighlight %}

	Mar 07 15:45:33 tiros chat[7684]: ^M
	Mar 07 15:45:33 tiros chat[7684]: ATDT*99#^M^M
	Mar 07 15:45:33 tiros chat[7684]: CONNECT
	Mar 07 15:45:33 tiros chat[7684]: -- got it
	Mar 07 15:45:33 tiros chat[7684]: send (^M)
	Mar 07 15:45:33 tiros pppd[7675]: Serial connection established.
	Mar 07 15:45:33 tiros pppd[7675]: Using interface ppp0
	Mar 07 15:45:33 tiros pppd[7675]: Connect: ppp0 <--> /dev/ttyUSB1
	Mar 07 15:45:34 tiros pppd[7675]: CHAP authentication succeeded
	Mar 07 15:45:34 tiros pppd[7675]: CHAP authentication succeeded

There should also be a new network interface named `ppp0` available.

References
====
- [GitHub: netctl][1]
- [ThinkWiki: Qualcomm Gobi 2000][3]
- [ArchWiki: Gobi Broadband Modems][4]

[1]: https://github.com/joukewitteveen/netctl
[2]: http://wiki.ubuntuusers.de/Mobiler_Datentransfer
[3]: http://www.thinkwiki.org/wiki/Qualcomm_Gobi_2000
[4]: https://wiki.archlinux.org/index.php/Gobi_Broadband_Modems

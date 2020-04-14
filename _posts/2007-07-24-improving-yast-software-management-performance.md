---
created: 1185240180
layout: post
redirect_from:
- article/12/
- node/12/
- improving-yast-software-management/
title: Improving YaST Software Management Performance
---
I noticed that ZMD runs for quite a while when your machine boots. ZMD is used to manage software through YaST. SUSE has its own software that works fine and does not appear to be such a machine hog as ZMD.

To shut of ZMD open a Konsole (ALT + F2 then type konsole) and enter the following.
<blockquote>chkconfig -s novell-zmd off</blockquote>After running the command I no longer experienced the resource hogging that ZMD appears to have caused.

If the SUSE Updater does not start after you reboot then start it manual from the menus.
<blockquote>System->Desktop Applet->openSUSE Update Applet
</blockquote>If you haven't installed SUSE yet I recommend <a href="http://boombatower.blogspot.com/2007/08/suse-installation-thoughts.html">this</a> instead.


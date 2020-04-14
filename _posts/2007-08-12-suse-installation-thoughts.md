---
created: 1186960260
layout: post
redirect_from:
- article/18/
- node/18/
- suse-installation-thoughts/
title: SUSE Installation Thoughts
---
<h3>Partitions</h3>

It is a good idea to partition your drive into a main partition for applications and system stuff, home driver, and swap partition. By doing this you will be able to re-install SUSE without touching your personal files.

A swap partition is used when your computer runs our of memory then it can use the hard driver space for extra memory. I usually do a gig swap partition.

<h3>Turn off ZMD</h3>

I have found that there is really no way to remove ZMD after installing SUSE. You can disable ZMD, but there isn't any real way to un-install it.

My recommendation is to never install it. When installing SUSE simple click the software link when that screen is available and set <i>Enterprise Software Management (ZENworks Linux Management)</i> to <i>Taboo</i> by right clicking and selecting <i>Taboo</i>.

<b>If you have already installed SUSE and just want to disable it please try <a href="/improving-yast-software-management">Improving Yast Software Management</a>.</b>

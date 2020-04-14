---
created: 1185240600
layout: post
redirect_from:
- article/13/
- node/13/
- get-rar-functionality/
title: Get RAR Functionality in SUSE 10.2
---
Coming from Windows and <a href="http://www.rarlab.com/">WinRAR</a> I attempted to figure out how to extract RAR files on SUSE and came up with the following solution.

Add the following source to YaST. If you are new to adding installation sources please refer to <a href="/adding-yast-installtion-sources">Adding YaST Installation Sources</a>.
<blockquote>htt://ftp-linux.cc.gatech.edu/pub/suse/suse/update/10.2</blockquote>Then goto <i>Software Management</i> and install <i>unrar</i>. Once completed you will be able to extract and view RAR contents through Konqueror just as you would with ZIP or TAR archive.

<b>Update</b>

You can now use <a href="http://software.opensuse.org/search?p=1&baseproject=ALL&q=unrar">SUSE software search</a> to find up-to-date unrar packages. Also note that KDE 4 on SUSE seems to automatically come packaged with it.

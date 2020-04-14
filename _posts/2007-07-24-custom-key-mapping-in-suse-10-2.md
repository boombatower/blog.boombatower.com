---
created: 1185253980
layout: post
redirect_from:
- article/14/
- node/14/
- custom-key-mapping-in-suse-102/
title: Custom Key Mapping in SUSE 10.2
---
I like mapping keys to tasks/applications that I use often. This can be done very easily in SUSE 10.2 as I will explain. For this example I will show you how to emulate the Windows plus E key functionality in SUSE.

Open up <span style="font-style: italic;">Input Actions</span> window. Menu->Settings->Regional &amp; Accessibility->Input Actions.

<a  href="/files/blogger/inputMenu.png"><img style="cursor: pointer;" src="/files/blogger/inputMenu.thumbnail.png" alt="" id="BLOGGER_PHOTO_ID_5090628478909682178" border="0" /></a>

Then add a new group for your custom actions. I added <span style="font-style: italic;">Windows Key</span> since I choose to map some Windows key combinations.

<a  href="/files/blogger/inputGroup.png"><img style="cursor: pointer;" src="/files/blogger/inputGroup.thumbnail.png" alt="" id="BLOGGER_PHOTO_ID_5090628848276869650" border="0" /></a>

I added the WIN+E that opens up home folder similar to Windows behavior. To do this add new action under your recently added group. Set the information to what is shown in the screenshot below.

<a  href="/files/blogger/home1.png"><img style="cursor: pointer;" src="/files/blogger/home1.thumbnail.png" alt="" id="BLOGGER_PHOTO_ID_5090629986443203106" border="0" /></a>

Next set the keyboard shortcut by clicking on the icon of a keyboard button as in the screen below.

<a  href="/files/blogger/home2.png"><img style="cursor: pointer;" src="/files/blogger/home2.thumbnail.png" alt="" id="BLOGGER_PHOTO_ID_5090630600623526450" border="0" /></a>

You should see the following.

<a  href="/files/blogger/home2.5.png"><img style="cursor: pointer;" src="/files/blogger/home2.5.thumbnail.png" alt="" id="BLOGGER_PHOTO_ID_5090631012940386882" border="0" /></a>

You can now press down the key combination that you wish to map. So for this example press and hold the Windows key and the E key. Now enter the location of your home folder. Your home folder should be /home/[username]/ where [username] is replaced by your computer username. For example user john would be /home/john/.

<a  href="/files/blogger/home3.png"><img style="cursor: pointer;" src="/files/blogger/home3.thumbnail.png" alt="" id="BLOGGER_PHOTO_ID_5090631493976724050" border="0" /></a>

This is just the beginning of what can be accomplished through the <i>Input Actions</i> dialog. You can setup conditions on when to run certain commands and when they apply. You can even setup mouse gestures.

---
created: 1249630928
layout: post
redirect_from:
- article/66/
- node/66/
- opensuse-one-click-lamp-drupal/
title: openSUSE one-click for LAMP and Drupal
---
<style type="text/css">
.install_btn {
float:right;
height:38px;
text-align:center;
width:135px;
}
.install_btn a {
display:block;
height:38px;
line-height:36px;
margin-left:32px;
text-decoration:none !important;
}
</style>
<div class="install_btn">
  <a href="http://download.opensuse.org/repositories/home:/boombatower/openSUSE_11.1/lamp-drupal.ymp">1-Click Install</a>
</div>
I decided to play around and see if I could create an <a href="http://en.opensuse.org/Standards/One_Click_Install">YaST Meta Package</a> for:
<ul>
<li>LAMP stack</li>
<li>Drupal/SimpleTest required PHP extensions</li>
<li>phpMyAdmin</li>
</ul>
After a quick read of the <a href="http://en.opensuse.org/Build_Service_Tutorial#Create_Patterns">Build Service/Tutorial</a> and a talk with cyberorg in IRC (#suse/#opensuse-buildservice) I was able to upload my pattern to the openSUSE build service.

It was quite simple as I expected since openSUSE's build service and such seem to be quite nice. I have plans to fix up some of my other Drupal packages, add more, and work on personal projects. If you use openSUSE and give it a try, let me know if it works or you have any feedback.

My repository is available at:
<ul>
<li><a href="http://download.opensuse.org/repositories/home:/boombatower/openSUSE_11.1/">http://download.opensuse.org/repositories/home:/boombatower/openSUSE_11.1/</a>.</li>
<li><a href="https://build.opensuse.org/project/show?project=home%3Aboombatower">https://build.opensuse.org/project/show?project=home%3Aboombatower</a></li>
</ul>

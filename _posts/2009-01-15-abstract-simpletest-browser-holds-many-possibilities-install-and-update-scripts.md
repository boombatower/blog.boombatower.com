---
created: 1232007378
layout: post
redirect_from:
- article/52/
- node/52/
- abstract-simpletest-browser-holds-many-possiblities-install-and-update-scripts/
title: 'Abstract SimpleTest browser holds many possibilities: install and update scripts'
---
<b>Situation</b>
The SimpleTest browser that is included in the Drupal 7 core is very powerful and I have found myself hacking in order to use it outside of SimpleTest on a number of occasions, as have others. In addition the DrupalWebTestCase is becoming rather bloated. There is an <a href="http://drupal.org/node/64866">issue to create pluggable backends for drupal_http_request</a> which could be cleanly done by abstracting the SimpleTest browser. The change would create a clean and more powerful API as well as open up a number of possibilities by having a browser built into Drupal core and thus is something worth working on.

<b>Solution</b>
A while back I did some <a href="http://drupal.org/node/340283">work towards abstracting the SimpleTest browser</a>, not only out of DrupalWebTestCase, but improving/cleaning the code and writing it with a pluggable backend layer. The code is quite far along and just needs a bit more refinement before it can replace the existing SimpleTest browser. The next step would be to replace drupal_http_request or make it a wrapper. After that, areas of core that parse HTML and such can be cleaned up and simplified using the browser.

<b>Possibilities</b>
With a browser in core there are a number of powerful tools that can very easily be added. For instance install and update scripts that can trigger remote servers!! Think of having a single script on a development machine that runs the update script on all specified sites!

Installing via a script can be very usefull not only for the obvious, but also for modules like the <a href="http://drupal.org/project/uts">Usability Testing Suite</a> and <a href="http://drupal.org/project/project_issue_file_review">Project Issue File Review</a> that have to make separate installations of Drupal and currently must maintain their own install scripts. Having an abstracted installation script is necessary for <a href="http://testing.drupal.org">testing.drupal.org</a> to be able to function with patches that make changes to the installer.

I plan to work on this further, but have been busy with other projects lately and my recent <a href="/bling-but-soon-i-will-see">video card experience</a>. I would like to hear some feedback from the community and possibly recruit some help.

<b>Related issues</b>
<ul>
<li><a href="http://drupal.org/node/64866">Pluggable architecture for drupal_http_request()</a></li>
<li><a href="http://drupal.org/node/340283">Abstract SimpleTest browser in to its own object</a></li>
<li><a href="http://drupal.org/node/330504">Create auto-install script</a></li>
</ul>

---
created: 1277417511
layout: post
redirect_from:
- article/77/
- node/77/
- general-quality-assurance-roadmap-and-request-for-help/
title: General quality assurance roadmap and request for help
---
As you may have seen in my <a href="/drupalcon-sf-quality-assurance-thoughts">previous post</a> I have been getting back into the swing of things after some time away. I have a number of projects that I need to attend to, but as always not enough time to work on them all. Bellow you will find a list of my most important projects that need attention. If you have time available and an interest in helping out I would appreciate any extra hands. A number of the items do not have links since I have yet to spec them out completely, but I will update this post as I fill them in.

<ul>
<li><b><a href="http://drupal.org/node/102102">Parse project .info files: present module list and dependency information</a></b> - needed in order for contrib testing to work 100% of the time and to move it out of beta so anyone can enable testing of their project.</li>
<li><b><a href="http://drupal.org/node/829852">PIFT 2.3</a> (drupal.org side of qa.drupal.org)</b> - In order to make the most of the qa framework a number of improvements need to be made, mainly options and information display on drupal.org so everyone can take advantage of all the features the system already provides.</li>
<li><b>PIFR 2.3 (qa.drupal.org and clients)</b> - A fair amount of testing of the current development code base, a few more bug fixes, and features are needed to round out the next release.</li>
<li><b>A few remaining cleanup items for SimpleTest in Drupal 7 core.</b></li>
<li><b><a href="http://drupal.org/project/simpletest">SimpleTest 7.x-2.x</a></b> - The branch will be used for continued development during Drupal 7 life cycle and already contains a number of improvements over the base Drupal 7 testing system. It was designed so that it can coincide with the Drupal cores framework and allow for tests to use either framework (one framework per module). The switch module, which no longer works (need to look into), allows you to switch between the contrib and core versions with a single click. The switch module also could be used by other projects attempting to replace core modules and can do so with a small amount of abstraction. With the next PIFT/PIFR release I plan to support projects that wish to use the 7.x-2.x API for testing.</li>
<li><b><a href="http://drupal.org/project/code_coverage">Code coverage</a></b> - Reports for all tests (patches or at least core/contrib commits).</li>
<li><b>Refactor/Rewrite SimpleTest for Drupal 8</b> - I have an extremely detailed roadmap in my head (and some paper notes) and a small code base that needs to be made public.</li>
<li><b>PIFR 3.x</b> - Same as above, lots of plans, but not very much publicly available yet.</li>
</ul>

Feel free to read through the issues, information, and code and let me know if you need any help getting started. Thanks!

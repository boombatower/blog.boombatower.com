---
created: 1252968573
layout: post
redirect_from:
- article/70/
- node/70/
- fresh-simpletest-backport-2.9/
title: Fresh SimpleTest backport - 2.9
---
Considering:
<ul>
<li>the last official backport was <a href="http://drupal.org/node/442446">April 23 2009</a></li>
<li>a number of <a href="/drupal-7-debug-and-simpletest-verbose">very cool features</a> have been added in core in addition to incremental changes</li>
<li>people have been asking</li>
<li>Drupal 7 is in "code slush"</li>
</ul>
it seems appropriate to perform another backport. I finished the bulk of the backport during Drupalcon Paris and have been tweaking and fixing bugs from feedback since then.

In order to ensure that all the new features from Drupal 7 were available it was necessary to create a patch against Drupal 6 core which needs to be applied before installation, as described in <a href="http://cvs.drupal.org/viewvc.py/drupal/contributions/modules/simpletest/INSTALL.txt?view=markup&pathrev=DRUPAL-6--2">INSTALL.txt</a>.

Please update and report any issues in the <a href="http://drupal.org/project/issues/simpletest">queue</a> and have fun with the new features and proper error reporting!

<div style="text-align: center">
<img src="/files/simpletest-2.9.png" alt="test run" />
</div>

---
created: 1223530079
layout: post
redirect_from:
- article/42/
- node/42/
- final-pieces-to-automated-patch-testing/
title: Final pieces to automated patch testing
---
I recently had a talk with <a href="http://drupal.org/user/22079">hunmonk</a> in IRC about what needs to be done to get <a href="http://drupal.org">drupal.org</a> to start sending patches to the <a href="/automated-patch-testing-(testing.drupal.org)-realized">new automated testing framework</a>. After almost an hour of discussion and thought we came to the following agreement. <i>(summarized from IRC log)</i>

<b>Initial plan</b>
<ul>
<li>PIFT which is already in place on <a href="http://drupal.org">drupal.org</a> will continue to be used to scan the issue queue and send patches to <a href="http://testing.drupal.org">testing.drupal.org</a>. (May need criteria updated.)</li>
<li>PIFT client will then be ported to Drupal 6.x and benefit from the addition of a hook as apposed to the current mechanism.</li>
<li>PIFR will be updated to implement the new PIFT hook in the ported Drupal 6.x client.</li>
</ul>

<b>Future plan</b>
<ul>
<li>Reporting back to <a href="http://drupal.org">drupal.org</a> will be discussed a bit further as there are several ways and opinions on how it should work both technically and from a workflow standpoint.</li>
<li>PIFR will send summary of testing results back to <a href="http://drupal.org">drupal.org</a> through PIFT.</li>
<li>PIFT will display the results on <a href="http://drupal.org">drupal.org</a>.</li>
</ul>

<b>Possible future</b>
<ul>
<li>PIFT may be further abstracted to allow for general distributed information sending between Drupal sites.</li>
<li>Reporting logic may be moved out of PIFT and into another module.</li>
</ul>

<b>Nomenclature:</b>
<ul>
<li>PIFT - <a href="http://drupal.org/project/project_issue_file_test">Project issue file testing platform</a></li>
<li>PIFR - <a href="http://drupal.org/project/project_issue_file_review">Project issue file review</a> (code for <a href="/automated-patch-testing-(testing.drupal.org)-realized">new automated testing framework</a>)</li>
</ul>

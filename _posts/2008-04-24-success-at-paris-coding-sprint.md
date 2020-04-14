---
created: 1209066577
layout: post
redirect_from:
- article/5/
- node/5/
- success-at-paris-coding-sprint/
title: Success at Paris Coding Sprint
---
The Paris sprint went off without a hitch.  We accomplished several goals that will move Drupal into its next stage of development.  Among the things accomplished was the <a href="http://drupal.org/cvs?commit=111812">addition of the SimpleTest module to Drupal core</a>.  Inclusion of a common testing suite with Drupal core will provide a basis for complete testing coverage which was stated as one of the <a href="http://buytaert.net/drupal-7-timeline">goals for Drupal 7</a>.  A necessary component of this initiative is a method for measuring the success of testing.  This will be done with "code coverage" tools which measure the percentage of code that is being tested.  To put all of this work to good use an automated testing farm is being created which will run the tests on all patches to core.  Work on these three areas was unified and pushed forward at the sprint.

An enormous amount of <a href="http://groups.drupal.org/node/9408">preparation</a> has been <a href="http://groups.drupal.org/node/10099">completed</a> over the past month and a half in order to prepare SimpleTest for inclusion in the core.  The work has included API cleanup and documentation, functional test writing, statistic collection, and unit test planning.  The first three objectives were finalized at the sprint before SimpleTest was added to core.  Major strides were made on the last objective at the sprint and was planned to become part of core within the next week or two. The whole idea of unit testing is still <a href="http://groups.drupal.org/node/10957">somewhat up in the air</a>.

Without the sprint the final pieces necessary to bring SimpleTest into the core would have taken much longer to finalize. <a href="/thanks-for-fundraising-paris-coding-sprint-unit-testing">Thanks again</a> to all those who sponsored my trip to the sprint.

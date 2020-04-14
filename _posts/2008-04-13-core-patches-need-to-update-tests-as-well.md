---
created: 1208120740
layout: post
redirect_from:
- article/4/
- node/4/
- core-patches-need-to-update-tests-as-well/
title: Core patches need to update tests as well
---
The current plan is for SimpleTest to become part of core in the near future. Once this happens the tests will need to be updated with patches to core. However, at this point SimpleTest is not part of core and whenever core changes break the tests someone, usually me, has to go through and figure out why they broke, make sure that the code they test doesn't actually have a bug, and fix them.

As we draw nearer to the Unit Testing Sprint we are attempting to plan and prepare for the sprint. Having to go through and fix tests whenever the core is changed can be quite time consuming. It would be helpful if we could begin the practice of updating tests along with the core before SimpleTest is officially part of core. Any help in this department would be much appreciated.

Recently a number of the tests broke due to changes in the core. If you would like to help please see the <a href="http://groups.drupal.org/node/9408">list of tests that <i>should</i> pass</a> and make sure that they still do. Specifically, it appears that commit <a href="http://drupal.org/cvs?commit=109998">#109998</a> has broken over four of the tests and it would be appreciated if others could take the initiative and fix those tests.

Thanks in advance.

---
created: 1211337041
layout: post
redirect_from:
- article/7/
- node/7/
- core-patches-need-to-be-tested/
title: Core patches need to be tested!
---
It has come to our, SimpleTest module developers, attention that the tests are breaking at a rapid rate.  Most of the tests that currently have <a href="http://tinyurl.com/689a6h">issues</a> passed when they were originally committed to the core.  The <a href="http://groups.drupal.org/node/9408">list of tests that passed</a> was used during the cleanup stages, but hasn't been changed since SimpleTest was added to core and stands as a record.

I previously posted "<a href="/core-patches-need-to-update-tests-as-well">Core patches need to update tests as well</a>" in preparation for SimpleTest becoming part of core.  Sadly it doesn't appear it was heeded.

After a long process of trial and error rolling back commits, <a href="http://webchick.net">webchick</a> found the <a href="http://drupal.org/node/240387#comment-838687">patch</a> that <a href="http://drupal.org/node/260499">broke the translation test</a>, and possibly others.  This would have saved everyone involved a lot of time had those who reviewed and wrote the patch simply run the tests on the patch, or better yet if <a href="http://testing.drupal.org">testing.drupal.org</a> were finished and implemented on <a href="http://drupal.org">drupal.org</a> so that it would happen automatically.

Regardless it is rather annoying to spend several weeks cleaning up the tests to see them broken by un-tested commits.  Before marking a patch as "ready to be committed" ensure that someone has run the suite of tests on it.  If that hasn't been done I would hope that the patch isn't committed.

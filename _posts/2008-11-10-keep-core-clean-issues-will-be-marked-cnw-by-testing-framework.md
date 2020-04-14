---
created: 1226277491
layout: post
redirect_from:
- article/45/
- node/45/
- keep-core-clean-issues-will-be-marked-CNW-by-testing-framework/
title: 'Keep core clean: issues will be marked CNW by testing framework'
---
The final updates to the testing framework have been <a href="http://drupal.org/node/329973">placed</a> on <a href="http://drupal.org">drupal.org</a>. The updates will allow issues to be marked as <i>code needs work</i> when the latest eligible patch fails testing.

After a bit of work fixing the latest bugs in core in order to make all tests pass, the testing system has been reset and should now be reporting back to drupal.org again, but this time marking issues as CNW!

Current Drupal 7 issue queue status:
```
267 Pending bugs (D7)
241 Critical issues (D7)
1116 Patch queue (D7)
388 Patches to review (D7) 
```

I hope to see some changes.

On the other side this means if a bug is committed to core that causes the tests to fail all patches tested will be marked as CNW.

<b>Make sure you wait for testing results on a patch before you mark it as <i>ready to be committed</i>.</b>


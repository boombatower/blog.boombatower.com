---
created: 1219364418
layout: post
redirect_from:
- article/39/
- node/39/
- simpletest-7.x-core-for-drupal-6/
title: SimpleTest 7.x core for Drupal 6
---
For those of you who don't follow Drupal 7 development, the <a href="/success-at-paris-coding-sprint">SimpleTest module was committed to core</a>. I have been maintaining the both core and contrib versions of the module and decided to back-port the enhancements made in core to contrib.

There are now two versions of <a href="http://drupal.org/project/simpletest">SimpleTest</a> available for Drupal 6.

<ul>
<li>1.x: Old code base that requires SimpleTest PHP library.</li>
<li>2.x: New code, with countless enhancements, back-ported from core.</li>
</ul>

Because of the back-ported 2.x version you can write your tests in the Drupal 7 format that is the future. It is highly recommenced that you convert tests to this new format as Drupal 7 will not support it and Drupal 5 and 6 may not support the old format much longer. Converting will also help because the documentation available is being converted to the new format and many new features are available.

If you are interested in starting to write tests or getting up to speed on recent development I have create a <a href="http://drupal.org/node/291740">getting started</a> document that will explains the different modules available and give you a brief history or changes.

If anyone is interested in helping me back-port the changes to 5.x that would be great. Better yet just get it working and submit a patch.

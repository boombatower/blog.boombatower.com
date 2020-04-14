---
created: 1235110544
layout: post
redirect_from:
- article/53/
- node/53/
- simpletest-never-sleeps/
title: SimpleTest never sleeps
---
I am sure by now most, if not all, are aware of the <a href="http://drupalbin.com/submitcode">drupal.org upgrade to Drupal 6</a> and may have noticed some of the changes. After the upgrade was completed I went ahead and posted my updated hook_test() patch to <a href="http://drupal.org/node/376129">Create hook_test(): move SimpleTest getInfo() out of test cases</a>. Upon posting I noticed that the file was uploaded to:

`http://drupal.org/files/`

instead of

```http://drupal.org/files/issues/```

That may not sounds like much of an issue, but it has a number of trickle effects. 
<ul>
<li>Files are not renamed properly. Meaning that files with the same name may exist, but in different directories.</li>
<li>Inconsistent data would be sent to <a href="http://testing.drupal.org">testing.drupal.org</a> that would have caused issues.</li>
<li>Confusing urls.</li>
</ul>
I talked with <a href="http://drupal.org/user/22079">Chad "hunmonk" Phillips</a> about it in IRC and discovered that is was due to the new File API. Drupal.org was put in maintenance mode, as many of you probably noticed, and Chad dove into the code. After discussing for a while a <a href="http://drupal.org/node/377650"><i>"fix"</i> was created</a>.

The moral of the story...yet another bug found due to SimpleTest <i>(the patch I was posting was related to SimpleTest)</i>. That of course is not to say it would not have been found soon enough, but SimpleTest indirectly found it first. :)

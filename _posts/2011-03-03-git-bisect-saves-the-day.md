---
created: 1299134472
layout: post
redirect_from:
- article/79/
- node/79/
- git-bisect-saves-the-day/
title: Git bisect saves the day
---
I have been working on a private project for quite some time using git.  Yesterday, I noticed that one of the views on the site was taking around 10 seconds to generate instead of less than a second like it used to. I scanned through the recent commits, scratched my head, and messed with a bunch of stuff, but to no avail. I reverted to a commit from over a month ago just for kicks and sure enough the view rendered quickly again. So I decided it was time to learn how to use git bisect.

From git documentation:
<blockquote>
Find by binary search the change that introduced a bug
</blockquote>

I had read in passing the general idea behind git bisect and that you could use a script that returned pass or fail to automate the process. After reading through the man page I confirmed that a script may be used and simply needs to return exits status code <i>0 for pass</i> and <i>1 for fail</i>. So I figured I could write a script that manually executes the view and check to see if the amount of time required was over a certain threshold. Interestingly it seems the view executes much faster using the script than inside a normal page request. Thus the threshold used is much lower then one might expect.

I created two files since I wanted to use `drush php-script` and follow the documentation's recommendation by placing the scripts outside the repository. The first script is a wrapper that simply changes to the directory in which Drupal is installed and then executes the drush command.

<strong>git_bisect.sh</strong>
```bash
#!/bin/bash
cd /path/to/drupal
drush php-script ~/check_view.php
```

<strong>check_view.php</strong>
```php
drupal_flush_all_caches();

$view = views_get_view('MY_CUSTOM_VIEW');
$view->set_arguments(array(3)); // Test data.
$start = microtime(TRUE);
$view->execute();
$stop = microtime(TRUE);

// 0: pass, 1: fail
$diff = $stop - $start;
$status = $diff > 0.3 ? 1 : 0; // Threshold.

var_dump($diff);
var_dump($status);

// If exit(0) is called drush still views it as abnormal shutdown and sets code
// to non-zero so only call when we want abnormal shutdown.
if ($status != 0) {
  exit(1);
}
```

My case was a bit more complex since code beyond a certain point was incompatible since I had to backup a related module to work with old revisions.
```
078d60ad18b73ec356436a7ea30528c95c9c4844 (bad)
3f1cfca83821a6b2d694cf228e5d8af3db20922f (good)
```

I ran the following inside the repository directory.
```bash
git bisect start 078d60ad18b73ec356436a7ea30528c95c9c4844 3f1cfca83821a6b2d694cf228e5d8af3db20922f --
git bisect run ~/git_bisect.sh
```

I ended up with the following result (-- indicates where I scrubbed data for privacy).
```
running /home/boombatower/git_bisect.sh
float(0.43191289901733)
int(1)
Drush command terminated abnormally due to an unrecoverable error.
Bisecting: 7 revisions left to test after this (roughly 3 steps)
[50dcca7e9cec514c2bcc24156cd8b4622eb2cd3e] -- message --
running /home/boombatower/git_bisect.sh
float(0.53287100791931)
int(1)
Drush command terminated abnormally due to an unrecoverable error.
Bisecting: 3 revisions left to test after this (roughly 2 steps)
[37e793d693a75a55470e6a92f5e3f30649ee2214] -- message --
running /home/boombatower/git_bisect.sh
float(0.18644404411316)
int(0)
Bisecting: 1 revision left to test after this (roughly 1 step)
[77ddb40b26fe2436cd7a15549109ffa9095d6995] -- message --
running /home/boombatower/git_bisect.sh
float(0.51487994194031)
int(1)
Drush command terminated abnormally due to an unrecoverable error.
Bisecting: 0 revisions left to test after this (roughly 0 steps)
[08d99c98f4b9d837775db47770bc125727d93dc6] -- message --
running /home/boombatower/git_bisect.sh
float(0.1781919002533)
int(0)
77ddb40b26fe2436cd7a15549109ffa9095d6995 is the first bad commit
commit 77ddb40b26fe2436cd7a15549109ffa9095d6995
Author: --
Date:   --

    -- message --

:040000 040000 a4d3a8cb990d0eff7a7dd87c941a3f12b768feaf 10ab97d60a9b03a56862828e0f9f66cf7f4ef6b4 M      --
:100644 100644 7254a3027edfb35e10b948a9dcd994a9fbdd44a3 0a8ef37931373929328fe6458bf1f595549d265a M      --
bisect run success
```

Sure enough the "first bad commit" was indeed the commit that caused the performance issue. Very cool!

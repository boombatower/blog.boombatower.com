---
created: 1240465998
layout: post
redirect_from:
- article/57/
- node/57/
- simpletest-6.x-2.8-fresh-backport/
title: SimpleTest 6.x-2.8 - fresh backport
---
I maintain a backport of Drupal 7 SimpleTest for use with Drupal 6 as the <a href="http://drupal.org/project/simpletest">SimpleTest module</a> 2.x branch. A fair amount of work goes into maintaining the code even though it is a backport.

Tonight I finished up a fresh backport of Drupal 7 and made a <a href="http://drupal.org/node/442446">release</a> along with a number of other changes. Please update to <a href="http://ftp.drupal.org/files/projects/simpletest-6.x-2.8.tar.gz">SimpleTest 6.x-2.8</a> and report any issues you find.

<b>Note to developers</b>

Since this release includes a fresh backport it also contains an API change that you need to be aware of. All <i>getInfo()</i> methods need to be changed to <i>static</i> before using this release. If you are a module developer or maintain tests internally please make sure you update them.

<u>Old</u>

```php
function getInfo() {
  return array(
    'name' => t('[name]'),
    'description' => t('[description]'),
    'group' => t('[group]'),
  );
}
```


<u>New</u>
```php
public static function getInfo() {
  return array(
    'name' => t('[name]'),
    'description' => t('[description]'),
    'group' => t('[group]'),
  );
}
```

---
created: 1250549263
layout: post
redirect_from:
- article/69/
- node/69/
- drupal-7-debug-and-simpletest-verbose/
title: 'Drupal 7: debug() and SimpleTest->verbose()'
---
Recently, I have made two major improvements to debugging in Drupal 7, the addition of a <a href="http://drupal.org/node/296574">general debug function</a> and a <a href="http://drupal.org/node/500280">verbose mode for SimpleTest</a>. The two additions make it much easier to debug problems quickly through the use of a consistent method. Take a look at what <a href="http://twitter.com/chx1975/status/3345691514">chx said via twitter</a>:

> Writing #drupal code? Check the new function debug(). Writing #drupal tests? Check $this->verbose(). And debug() works too. AWESOME!

<b>General debug function</b>

The general debug function can be used at any point after Drupal is bootstrapped, although the limitation may be removed in the future. The function provides a very simple wrapper to dump data through the use of `var_export()` and `print_r()`. When used normally it will display data based on the "Logging and errors" settings provided in Drupal 7 core. If using a dev version the debug information will be displayed using `drupal_set_message()` as shown in the screenshot below.

<div style="text-align: center"><img src="/files/debug-normal.png" alt="debug normal" /></div>

The exciting part about the new `debug()` function is that it also works during testing. The `debug()` function can be placed inside the test itself or in any other part of Drupal and it will be picked up and displayed in the test results as shown below.

<div style="text-align: center"><img src="/files/debug-test.png" alt="debug test" /></div>

<b>SimpleTest verbose mode</b>

Another exciting new debugging tool that is extremely useful when writing tests is the new verbose mode for Drupal 7 SimpleTest. The verbose mode can be enabled on the SimpleTest settings page.

<div style="text-align: center"><img src="/files/verbose-setting.png" alt="verbose setting" /></div>

Once enabled SimpleTest will automatically record the page as it was seen by the SimpleTest browser after each `drupalGet()` and `drupalPost()` call. A link is then placed in the test results that will display the page the browser saw and some meta data related to the request.

<div style="text-align: center"><img src="/files/verbose-link.png" alt="verbose link" /></div>

<b>Page 1</b>

<div style="text-align: center"><img src="/files/verbose-page1.png" alt="verbose page1" /></div>

<b>Page 2</b>

<div style="text-align: center"><img src="/files/verbose-page2.png" alt="verbose page2" /></div>

<b>Manual verbose</b>

In addition to the automatic message provided by SimpleTest custom verbose data may be dumped using `DrupalWebTestCase->verbose()` which can be used in a test as shown.

```php
$this->verbose($data);
```

If the data to be dumped in not available in the test, but in the code being tested a function is provided that may be accessed by including the `DrupalWebTestCase` as shown below.

```php
require_once drupal_get_path('module', 'simpletest') . '/drupal_web_test_case.php';
simpletest_verbose($data);
```

<b>Summary</b>

By adding these debugging tools to Drupal 7 the developer experience involved in writing a test has been greatly improved. These methods can still be improved and as such please feel free to file issues in the <a href="http://drupal.org/project/issues/drupal?status=Open&component=simpletest.module">Drupal 7 SimpleTest issue queue</a>. Also note that this work was sponsored by Acquia as part of my <a href="/acquia-internship">Summer Internship</a>.

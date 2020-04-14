---
created: 1428096708
layout: post
redirect_from:
- article/98/
- node/98/
- drupal-testbot-command-line-tool/
title: Drupal testbot command line tool
---
Are you a developer familiar with the patch submission workflow?

* load the issue page in your browser
* add some helpful text describing the patch
* create a patch file
* upload it to the issue
* change the issue status to 'Needs review'
* wait 20 minutes (at best) to find out if the tests pass or fail with your patch

If the tests fail, then you get to do it all over again, including making possible revisions to the issue summary.

Ever wanted to run the tests while iterating on a change, but are reluctant due to:

* insufficient local machine resources or configuration to run tests efficiently
* inconsistent results compared to the official testbot
* disruption of your 'creative coding moments' due to the patch submission workflow
* long test runtime on qa.d.o

Well you may be in luck.

What if you could type `drush testbot` to have the changes in your working branch submitted and tested as a patch against Drupal? And be able to view the results in five minutes or less? If this sounds interesting, then install the `drush testbot` command file (see instructions) and take it for a spin.

## Suggested usage

If your changes only affects a single module (or a few), then an assessment of your changes can often be had by simply running the tests for that module (or those modules). Once you know that your changes do not break the immediate context, then the entire test suite can be run before posting the patch on the issue queue.

You can do so by including a list of test classes to run (see examples); in so doing you might reduce your response time to a matter of seconds.

## Drupal 8 examples

Run the command from the working directory with your code changes. The default test environment for Drupal 8 is mysql 5.5 and php 5.4. In lieu of setting properties on the command line, add them to a '.testbot' file in the repository root directory (i.e. the same directory that contains the .git directory).

<table>
  <tr>
    <th>Description</th>
    <th>Command</th>
  </tr>
  <tr>
    <td>All defaults
(without .testbot file)</td>
    <td><code>drush testbot</code></td>
  </tr>
  <tr>
    <td>Using properties from .testbot file</td>
    <td><code>drush testbot</code></td>
  </tr>
  <tr>
    <td>Single test class</td>
    <td><code>drush testbot --properties='{"classes":["Drupal\taxonomy\Tests\TermTest"]}'</code></td>
  </tr>
  <tr>
    <td>Alternate environment and multiple test classes</td>
    <td><code>drush testbot --database='mongodb-2.6' --php-version='php-5.5'
--properties='{"classes":["Drupal\node\Tests\NodeQueryAlterTest",
"Drupal\node\Tests\NodeRevisionsTest"]}'</code></td>
  </tr>
</table>


## Drupal 7 examples

The default test environment for Drupal 7 is mysql 5.5 and php 5.3. The command syntax matches that shown above with the addition of the 'branch:7.x' parameter to the properties. An example of testing two classes is:

```
drush testbot --properties='{"branch":"7.x","classes":["NodeQueryAlter","NodeRevisionsTestCase"]}'
```

## Instructions

Point your browser to [https://github.com/reviewdriven/testbot](https://github.com/reviewdriven/testbot) and see the README file for installation instructions.

## Caveat

If our test queue is empty when you submit a patch (and do not specify a list of classes to test), the actual response time for a Drupal 8 patch will be under ten minutes based on existing test suite and available hardware at the time of this writing. If the queue is full, we may not run your patch and will let you know.

## Final thoughts

If you have questions, would like to offer constructive comments or suggestions, or want to let us know that you found the tool useful, please post in the [github issue queue](https://github.com/reviewdriven/testbot/issues).

The testbot is provided as a complimentary service to the Drupal community. No financial assistance is received from the Drupal Association (to defray even the hardware costs). Unlike the current testbot which runs on the most powerful compute instances offered by Amazon Web Services, these tests are run on hardware with roughly 25% the processing power yet return results in under half the time.

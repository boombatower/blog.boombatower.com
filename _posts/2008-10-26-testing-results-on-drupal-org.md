---
created: 1225004996
layout: post
redirect_from:
- article/44/
- node/44/
- testing-results-on-drupal.org/
title: Testing results on drupal.org!
---
Today <a href="http://testing.drupal.org">testing.drupal.org</a> began sending results back to  <a href="http://drupal.org">drupal.org</a>! The results are displayed under the file they relate to. The reporting will not trigger a change in issue status if the testing fails, as is the future plan. That will be turned on when the automated testing framework has proven to be stable.

The following is an example of what a [pass result look like on drupal.org](http://drupal.org/node/180379#comment-739254).

<img src="/files/reporting.png" alt="pass example" />

The following is an example of what a [fail result look like on drupal.org](http://drupal.org/node/200185#comment-752032).

<img src="/files/reporting2.png" alt="fail example" />

Testing.drupal.org also provides detailed <a href="http://testing.drupal.org/pifr/stats">stats on patch results</a> and <a href="http://testing.drupal.org/pifr/status">status</a>. An example of patch result stats is as follows.

<img src="/files/testing.drupal.org-summary-stats.png" alt="stats example" />

<b>Early success</b>

Before the actual reporting was turned on the automated testing framework discovered two core issues. One was a partial commit that caused a <a href="http://drupal.org/node/323182#comment-1066946">PHP syntax error</a>. The other was a <a href="http://drupal.org/node/323372">bug with the installer</a>. I hope to see the results make patch reviewing much faster and easier.

<b>Your help</b>

<a href="http://drupal.org/project/issues/project_issue_file_review">Please report any test result anomalies in the PIFR queue</a>.

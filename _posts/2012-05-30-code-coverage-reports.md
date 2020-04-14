---
created: 1338386269
layout: post
redirect_from:
- article/89/
- node/89/
- code-coverage-reports/
title: Code coverage reports
---
Today, I made public a whole bunch of commits I made while improving the [Code coverage](http://drupal.org/project/code_coverage) project for use with [ReviewDriven](http://reviewdriven.com/). You may remember the screenshot I included in [Part 2: Breathing new life into the testbot](/breathing-new-life-into-the-testbot) which is where this is coming from. Thanks to the improvements the module now actually works and is much more efficient, polished, accurate, and much more. With that in mind I am proud to announce the [first stable release](http://drupal.org/node/1608240) of the project targeted at [Drupal 7.14](http://drupal.org/node/1558424).

Using the module you can get coverage reports for individual page executions or any combination of tests. Code coverage reports can be extremely useful in determining areas of code that are not tested at all or provide a snapshot of the code involved in generating a page.


## Pages

Code coverage can be recorded by adding `?code_coverage=true` to the end of a page URL. After the page has completed execution a linked will be placed at the bottom of the HTML which will display outside the page style.

![Coverage page](/files/coverage_page.png)

The link will open the coverage report generated for that page request. The report will include all the files that where loaded during the execution of the page.


## Tests

Similarly, a link is provided for the code coverage recorded during a test run.

![Coverage test](/files/coverage_test.png)


## Reports

The report includes two parts: 1) the summary or index, and 2) the line-by-line coverage information. The links described above point to the summary of the coverage information.

![Coverage summary](/files/coverage_summary.png)

The links in the summary point to the line-by-line coverage information overlayed on the corresponding code. The colors indicate the following.

- green = executed
- red = not executed
- gray = ignored (or non-executable)

![Coverage example](/files/coverage.png)


## Filters

The coverage scope may be filtered to focus on improving coverage for a particular module/file/directory. Reducing the scope will also improve the coverage recording performance which may be useful when when dealing with large tests.

![Coverage filter](/files/coverage_filter.png)


## Future

I have already integrated the Code coverage project with Conduit (the open source ReviewDriven platform) which will be replacing the current system running [qa.drupal.org](http://qa.drupal.org/). The plan is to get the new platform up and running in parallel with the current system at which time regular coverage runs against core (and contrib projects) can be made publicly available.

<style type="text/css">
.node img {
  margin: 5px 50px;
}
</style>

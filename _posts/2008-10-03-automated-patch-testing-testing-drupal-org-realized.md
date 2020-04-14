---
created: 1223070205
layout: post
redirect_from:
- article/41/
- node/41/
- automated-patch-testing-(testing.drupal.org)-realized/
title: Automated patch testing (testing.drupal.org) realized
---
The idea of automated testing using SimpleTest has been around almost as long as the <a href="http://drupal.org/project/simpletest">SimpleTest module</a> itself. A movement to accomplishing this has been centered at <a href="http://testing.drupal.org">testing.drupal.org</a> and the following projects:

<ul>
<li><a href="http://drupal.org/project/simpletestauto">SimpleTest auto</a></li>
<li><a href="http://drupal.org/project/project_issue_file_test">Project issue file testing platform</a></li>
<li><a href="http://drupal.org/project/Drupaltestbed">Test driven development infrastructure</a></li>
</ul>

<h3><b>Testing.drupal.org</b></h3>

The goal is to have all patches against Drupal 7 core, over 1000 currently, automatically tested to ensure that they apply and pass the suite of tests which consists of over 5500 individual assertsions for about <a href="http://acquia.com/files/test-results/index.html">70% code coverage</a>. Completing this goal has been long in the making by a number of individuals, who have been focused on Drupal 4.7, 5, and 6, but now appears to be realized. Being heavily involved in SimpleTest development, I have taken on the task of finishing up the automated testing framework to be compatible with Drupal 7 and SimpleTest core.

<b>Hurdles</b>

The automated testing framework was designed to support Drupal 4-7 through a 5.x module. This approach worked well, but with the recent explosion in SimpleTest 7.x development the workflow and design has been changed.

Changes from previous SimpleTest version to 7.x:

<ul>
<li>The <a href="http://simpletest.org/">SimpleTest PHP library</a> is no longer in use.</li>
<li>The XML reporter is no longer available, and is necessary for transferring results from test slave to testing master server.</li>
<li>The Shell script to run tests does not work properly in that it returns different results than the web interface.</li>
<li>The Drupal 7.x SimpleTest framework does not support META refresh and thus cannot run batch API pages necessary for running the tests and installing.</li>
<li>The Drupal 7 installer does not auto detect JavaScript support and does not work without JavaScript. (<a href="http://drupal.org/node/313606">issue</a>)</li>
<li>The XML output, over 2.7MB, is too large to transfer between servers and will only continue to grow with more tests.</li>
</ul>

<b>Goal</b>

<img src="/files/workflow.png" alt="workflow" style="float: right" />
The desired workflow is described below.

<u>Steps</u>
<ol>
<li>A file is attached to an issue at drupal.org.</li>
<li>The system determines if the file is a patch/diff and fits the proper criteria.</li>
<li>The file and issue information is sent to testing.drupal.org.</li>
<li>The file is added to the testing queue.</li>
<li>When a testing slave is available the next file in the queue is sent to be tested.</li>
<li>Once the slave has completed the testing the result is sent back to testing.drupal.org.</li>
<li>The result is then store and then the next file in the queue is sent to the slave.</li>
</ol>

This will provide a very consistent way to test files that will allow for scaling in the event that additional servers are necessary to deal with the patch load.

<b>Solution</b>

The previous framework is described at http://testing.drupal.org. The new framework is based heavily off the old framework, but runs in Drupal 6, has been re-written, and is better suited for running tests against Drupal 7.

The re-written code resides as <a href="http://drupal.org/project/project_issue_file_review">Project Issue File Review</a>. The single module can act as any combination of the three roles: project server, testing master server, and testing slave. That means that the current single server that testing.drupal.org has available can act as both the testing master server and a testing slave. This setup can encompass internal business testing on a single server as well.

The module addresses the hurdles in several ways.

<ul>
<li>A patch is applied to Drupal 7 HEAD that converts the output from the standard HTML interface to an XML dump.</li>
<li>Instead of using the shell script the code uses SimpleTest with a <a href="http://drupal.org/node/316344">patch</a> that adds support for meta refresh.</li>
<li>When installing Drupal 7 HEAD the batch API is forced to use META refresh.</li>
<li>The XML output is gzipped to allow for it to transfered between slave and testing master server.</li>
</ul>

<b>Advantages</b>

Some may wonder why I re-wrote the existing code, there are several reasons.
<ul>
<li>Drupal.org is being <a href="http://drupal.org/node/295037">redesigned</a> in which it will also be ported to Drupal 6 so the testing framework might as well lead the way and not become a Drupal 5 dependency.</li>
<li>Solution that is tailored towards the SimpleTest 7.x workflow.
<ul>
<li>The previous framework kept each test installation around. Since SimpleTest no longer works on the active database all that would be seen is a base Drupal installation and is thus useless to keep around.</li>
<li>Removes SimpleTest library dependency.</li>
<li>Removes SimpleTest module download dependency during Drupal HEAD installation as it is now in core.</li>
<li>Code logic built around the community's desires for testing.drupal.org. (explained further bellow)</li>
</ul>
</li>
<li>File results are displayed in relation to the node they originated from which makes for easy relation between drupal.org and testing.drupal.org. As apposed to the previous system that assigned a random ID to each file tested. This setup still allows for multiple project servers with the same issue node IDs.</li>
<li>Files are sent for review immediately after being posted. (further explained below)</li>
<li>I much prefer working in Drupal 6 and when going through the trouble of porting why not do a bit or clean-up/re-write.</li>
</ul>

<b>What to expect</b>

<u>Summary of files per node</u>

<a href="/files/file_summary.png"><img src="/files/file_summary.png" alt="file summary" style="float: right; width: 499px; height: 77px;" /></a>
To view a list of all the files reviewed for an individual node you would enter a URL like: `http://testing.drupal.org/pifr/view/1/9` where 1 is the project server ID, could be replaced by textual "drupal.org", and 9 is the issue node ID. All the files that testing.drupal.org is aware of will be displayed along with their current status and testing result. The possible status values are: Queued, Sent to slave #, and Reviewed. This allows developers to check on a files testing status at any time. Although not displayed the comment ID is also stored for future auto commenting back on drupal.org issues.

<u>Detailed test results</u>

<a href="/files/file_results.png"><img src="/files/file_results.png" alt="file summary" style="float: right; width: 491px; height: 178px;" /></a>Upon clicking "View results" the detailed assertions will be displayed from the tests run. Currently this consists of a dump by test case. In the future this will hopefully look more the like the standard SimpleTest interface in Drupal 7. That will be handled in core when the test results layer is abstracted. I have already <a href="http://drupal.org/node/250047">created a patch</a> to begin this with the separation of test selection and test results. Once completed I will attempt to further cleanup the results display to work nicely for testing.drupal.org and any other modules that wish to integrate with SimpleTest. That will also allow this results page to be easily updated with the latest improvements to the SimpleTest results display.

<u>Status summary</u>

<a href="/files/server_status.png"><img src="/files/server_status.png" alt="file summary" style="float: right; width: 499px; height: 96px;" /></a>In addition to checking the status on individual files the overall status of the queue and testing slaves can be viewed by entering the URL: `http://testing.drupal.org/pifr/status`. The operations will only be visible to administrators. This will give an a general feel for the load being put on the testing infrastructure and allow for easy reset of a slave server when they crashes.

<b>What is left</b>

There are several things left to be done, some of which require the community's involvement.

<ul>
<li>Drupal 7 HEAD does not pass all tests. That means that when testing.drupal.org is turned on all patches will fail. That defeats the purpose of testing.drupal.org to some degree. Anyone can help to fixing the tests or issues related to the failures. Sadly it used to have 100% passes.</li>
<li><a href="http://drupal.org/node/316344">Add meta refresh support to SimpleTest</a> patch needs to make it in.</li>
<li><a href="http://drupal.org/node/250047">Rework the SimpleTest results interface</a>. The clean-up of backend code needs to complete and committed. The abstratction of the testing results code is dependent on this clean-up.</li>
<li>Testing results code needs to be abstracted once the clean-up of backend code gets committed.</li>
<li>Determine the proper criteria and workflow for choosing what patches to test and how to report back. (more detail below)</li>
<li>Code review would be nice, but lets focus on critical issues and worry about minors ones once testing.drupal.org is up and running.</li>
</ul>

<u>Patch criteria and workflow</u>

This is the one area that needs to be flushed out. As many may have noticed when testing.drupal.org was turned on to simply test if patch applied to the current development branch, refereed to as HEAD, it created a number of <a href="http://drupal.org/node/198452#comment-1025797">issues</a>. The following are issues that need to be resolved.

<ul>
<li>When should patches be sent to testing.drupal.org from drupal.org: right away or wait for cron?</li>
<li>If newer patch is posted when test results are returned should the auto comment still be sent?</li>
<li>Should the workflow be amended to have something like patch(tested) between patch(needs review) and patch(review &amp; tested by the community).</li>
<li>What should comment text contain? Link to testing results, message, both, etc?</li>
</ul>

One advantage with sending patches right when they are posted is that the logic about when to comment back can be put on testing.drupal.org as opposed to having a shared more complex logic on both drupal.org and testing.drupal.org. Since all that is sent is the node ID, comment ID, and file URL it shouldn't be a load on drupal.org.

Comments are appreciated, but keep in mind that the code is still being developed and there are a few things I will add before attempting to install on testing.drupal.org. Part of which is a Drupal 5 piece of code to send files, I might just modify the previous framework for that.

---
created: 1240373951
layout: post
redirect_from:
- article/56/
- node/56/
- automated-testing-system-2.0-final-steps/
title: Automated Testing System 2.0 - Final Steps
---
During the last several months I put a substantial amount of work into improving the <a href="http://testing.drupal.org">Automated Testing System</a>. Future posts will describe the exciting new features and the benefits to the community. If interested, a brief overview of some of the requirements can be found in the <a href="http://drupal.org/node/360904">PIFR</a> and <a href="http://drupal.org/node/361393">PIFT</a> issue queues.

For additional background, the original thoughts can be found at the following links:
<ul>
<li><a href="/the-future-of-automated-patch-testing">The Future of Automated Patch Testing</a></li>
<li><a href="/testbed-design-change-and-development">Testbed design change and development</a></li>
<li><a href="/automated-testing-system-development-clarification">Automated Testing System Development Clarification</a></li>
</ul>

<b>Final steps</b>

There a number of steps that need to be completed before the Drupal community can reap the benefits of the new system.

<ol>
<li>Security review of rewritten <a href="http://drupal.org/project/issues/project_issue_file_test">Project Issue File Test (PIFT)</a> module that integrates with the <a href="http://drupal.org/project/issues/project">project</a> module on <a href="http://drupal.org">drupal.org</a>.</li>
<li>Someone familiar with SQLite, possibly one of the <a href="http://cvs.drupal.org/viewvc.py/drupal/drupal/MAINTAINERS.txt?view=markup">D7 maintainers</a>, needs to write a PIFR DB driver to implement the required methods. MySQL and PostgresSQL have already been completed and can be used as examples. The driver is relatively simple, but will require manually connecting to SQLite since PIFR runs in D6 which does not support SQLite.</li>
<li>Update <a href="http://drupal.org/project/testing_server_setup">testing client setup/installation script</a> where necessary.</li>
<li>Deploy current development system to project.drupal.org and the create a parallel testing client network.</li>
<li>Freeze the current test client network and extract the <a href="http://drupal.org/node/416896#comment-1428316">test ID map</a> for use in drupal.org upgrade.</li>
<li>Upgrade and finalize test client network and test server (<a href="http://testing.drupal.org">testing.drupal.org</a>). Possibly move testing.drupal.org under the drupal.org infrastructure.</li>
<li>Confirm upgraded testing network is functional.</li>
<li>Plan for approximately 15 minutes of downtime on drupal.org.</li>
<li>Update PIFT code and run data update using extracted test ID map from #4 on drupal.org during downtime.</li>
<li>Watch deployed system closely and solicit community feedback and bug reports.</li>
<li>Request additional hardware to use as community test clients (to allow for future expansion into testing contributed modules).</li>
</ol>

<b>Future</b>

Once the second generation framework is in place and running smoothly I will begin work on finishing the last pieces required to allow for testing of contributed modules (D6 and D7) and Drupal 6 core. I will be writing more on the new features and UX improvements to be looking for in during the upcoming deployment.

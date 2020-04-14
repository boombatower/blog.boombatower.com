---
created: 1423788218
layout: post
redirect_from:
- article/97/
- node/97/
- woes-testbot/
title: The woes of the testbot
---
For those not familiar with me, a little research should make it clear that I am the person behind the testbot deployed in 2008 that has revolutionized Drupal core development, stability, etc. and that has been running tens of thousands of assertions with each patch submitted against core and many contributed modules for 6 years.

My intimate involvement with the testbot came to a rather abrupt and unintended end several years ago due to a number of factors (which only a select few members of this community are clearly aware). After several potholes, detours, and bumps in the road, it became clear to me the impossibility of maintaining and enhancing the testbot under the policies and constraints imposed upon me.

Five years ago we finished writing an entirely new testing system, designed to overcome the technical obstacles of the current testbot and to introduce new features that would enable an enormous improvement in resource utilization that could then be used for new and more frequent QA.

Five years ago we submitted a proposal to the Drupal Association and key members of the community for taking the testbot to the next level, built atop the new testing system. This proposal was ignored by the Association and never evaluated by the community. The latter is quite puzzling to me given:

* the importance of the testbot
* the pride this open source community has in openly evaluating and debating literally *everything* (a healthy sentiment especially in the software development world)
* I had already freely dedicated years of my life to the project.

The remainder of this read will:

* list some of the items included in our proposal that were dismissed with prejudice five years ago, but since have been adopted and implemented
* compare the technical merits of the new system (ReviewDriven) with the current testbot and a recent proposal regarding "modernizing" the testbot
* provide an indication of where the community will be in five years if it does nothing or attempts to implement the recent proposal.

This read will not cover the rude and in some cases seemingly unethical behavior that led to the original proposal being overlooked. Nor will this cover the roller coaster of events that led up to the proposal. The intent is to focus on a technical comparison and to draw attention to the obvious disparity between the systems.

# About Face

Things mentioned in our proposal that have subsequently been adopted include:

* paying for development primarily benefiting drupal.org instead of clinging to the obvious falacy of "open source it and they will come"
* paying for machine time (for workers) as EC2 is regularly utilized
* utilizing proprietary SaaS solutions (Mollom on groups.drupal.org)
* automatically spinning up more servers to handle load (e.g. during code sprints) which has been included in the "modernize" proposal

# Comparison

The following is a rough, high-level comparison of the three systems that makes clear the superior choice. Obviously, this comparison does not cover everything.

<style type="text/css">
table#testbot-comparison td {
  border: 1px solid white;
}
table#testbot-comparison  td:nth-child(2),
table#testbot-comparison  td:nth-child(3),
table#testbot-comparison  td:nth-child(4) {
  width: 33%
}
table#testbot-comparison  tr:nth-child(1),
table#testbot-comparison  td:nth-child(1) {
  font-weight: bold;
  font-size: 120%;
}
table#testbot-comparison  td:nth-child(2) {
  background-color: #FFCC00;
}
table#testbot-comparison  td:nth-child(3) {
  background-color: #D46A6A;
  color: white;
}
table#testbot-comparison  td:nth-child(4) {
  background-color: #55AA55;
  color: white;
}
</style>

<table id="testbot-comparison">
  <tr>
    <td></td>
    <td>Baseline</td>
    <td>Backwards modernization</td>
    <td>True step forward</td>
  </tr>
  <tr>
    <td>System</td>
    <td>Current qa.drupal.org</td>
    <td>"Modernize" Proposal</td>
    <td>ReviewDriven</td>
  </tr>
  <tr>
    <td>Status</td>
    <td>It's been running
for over 6 years</td>
    <td>Does not exist</td>
    <td>Existed 5 years ago at ReviewDriven.com</td>
  </tr>
  <tr>
    <td>Complexity</td>
    <td>Custom PHP code and Drupal

Does not make use of contrib code</td>
    <td>Mish mash of languages and environments: ruby, python, bash, java, php, several custom config formats, etc.<br><br>

Will butcher a variety of systems from their intended purpose and attempt to have them all communicate<br><br>

Adds a number of extra levels of communication and points of failure</td>
    <td>Minimal custom PHP code and Drupal<br><br>

Uses commonly understood contrib code like Views
</td>
  </tr>
  <tr>
    <td>Maintainability</td>
    <td>Learning curve but all PHP</td>
    <td>Languages and tools not common to Drupal site building or maintenance<br><br>

Vast array of systems to learn and the unique ways in which they are hacked</td>
    <td>Less code to maintain and all familiar to Drupal contributors</td>
  </tr>
  <tr>
    <td>Speed</td>
    <td>Known; gets slower as test suite grows due to serial execution</td>
    <td>Still serial execution and probably slower than current as each separate system will add additional communication delay</td>
    <td>An order of magnitude faster thanks to concurrent execution<br><br>

Limited by the slowest test case<br><br>

*See below</td>
  </tr>
  <tr>
    <td>Extensibility (Plugins)</td>
    <td>Moderately easy, does not utilize contrib code so requires knowledge of current system</td>
    <td>Several components, one on each system used<br><br>

New plugins will have to be able to pass data or tweak any of the layers involved which means writing a plugin may involve a variety of languages and systems and thus include a much wider breadth of required knowledge</td>
    <td>Much easier as it heavily uses commons systems like Views<br><br>

Plugin development is almost entirely common to Drupal development:<br>
define storage: Fields<br>
define display: Views<br>
define execution: CTools function on worker<br><br>

And all PHP</td>
  </tr>
  <tr>
    <td>Security</td>
    <td>Runs as same user as web process</td>
    <td>Many more surfaces for attack and that require proper configuration</td>
    <td>Daemon to monitor and shutdown job process, lends itself to Docker style with added security</td>
  </tr>
  <tr>
    <td>3rd party integration</td>
    <td>Basic RSS feeds and restricted XML-RPC client API</td>
    <td>Unknown</td>
    <td>Full Services module integration for public, versioned, read API and write for authorized clients</td>
  </tr>
  <tr>
    <td>Stability</td>
    <td>When not disturbed, has run well for years, primary causes of instability include ill-advised changes to the code base<br><br>

Temporary and environment reset problems easily solved by using Docker containers with current code base</td>
    <td>Unknown but multiple systems imply more points of failure</td>
    <td>Same number of components as current system<br><br>

Services versioning which allows components to be updated independently<br><br>

Far less code as majority depends on very common and heavily used Drupal modules which are stable<br><br>

2-part daemon (master can react to misbehaving jobs)<br><br>

Docker image could be added with minimal effort as system (which predates Docker) is designed with same goals as Docker</td>
  </tr>
  <tr>
    <td>Resource utilization</td>
    <td>Entire test suite runs on single box and cannot utilize multiple machines for single patch</td>
    <td>Multiple servers with unshared memory resources due to variety of language environments<br><br>

Same serial execution of test cases per patch which does not optimally utilize resources</td>
    <td>An order of magnitude better due to concurrent execution across multiple machines<br><br>

Completely dynamic hardware; takes full advantage of available machines.<br><br>

*See below</td>
  </tr>
  <tr>
    <td>Human interaction</td>
    <td>Manually spin up boxes; reduce load by turning on additional machines</td>
    <td>Intended to include automatic EC2 spin up, but does not yet exist; more points of failure due to multiple systems</td>
    <td>Additional resources are automatically turned on and utilized </td>
  </tr>
  <tr>
    <td>Test itself</td>
    <td>Tests could be run on development setup, but not within the production testbot</td>
    <td>Unknown</td>
    <td>Yes, due to change in worker design.<br><br>

A testbot inside a testbot! Recursion!</td>
  </tr>
  <tr>
    <td>API</td>
    <td>Does the trick, but custom XML-RPC methods</td>
    <td>Unknown</td>
    <td>Highly flexible input configuration is similar to other systems built later like travis-ci<br><br>

All entity edits are done using Services module which follows best practices</td>
  </tr>
  <tr>
    <td>3rd party code</td>
    <td>Able to test security.drupal.org patches on public instance</td>
    <td>Unknown, but not a stated goal</td>
    <td>Supports importing VCS credentials which allows testing of private code bases and thus supports the business aspect to provide as a service and to be self sustaining<br><br>

Results and configuration permissioned per user to allow for drupal.org results to be public on the same instance as private results</td>
  </tr>
  <tr>
    <td>Implemented plugins</td>
    <td>Simpletest, coder</td>
    <td>None exist</td>
    <td>Simpletest, coder, code coverage, patch conflict detection, reroll of patch, backport patch to previous branch</td>
  </tr>
  <tr>
    <td>Interface</td>
    <td>Well known; designed to deal with display of several 100K distinct test results; lacks revision history; display uses combination of custom code and Views</td>
    <td>Unknown as being built from scratch and not begun<br><br>

Jenkins can not support this interface (in Jenkins terminology multiple 100K jobs) so will have to be written from scratch (as proposal confirms and was reason for avoiding Jenkins in past)<br><br>

Jenkins was designed for small instances within businesses or projects, not a large central interface like qa.drupal.org</td>
    <td>Hierarchical results navigation from project, branch, issue, patch<br><br>

Context around failed assertion (like diff -u)<br><br>

Minimizes clutter, focuses on results of greatest interest (e.g. failed assertions); entirely built using Views so highly customizable<br><br>

Simplified to help highlight pertinent information (even icons to quickly extract status)<br><br>

Capable of displaying partial results as they are concurrently streamed in from the various workers</td>
  </tr>
</table>


# Speed and Resource Utilization

Arguably one of the most important advantages of the ReviewDriven system is concurrency. Interestingly, after seeing inside Google I can say this approach is far more similar to the system Google has in place than Jenkins or anything else.

Systems like Jenkins and especially travis-ci, which for the purpose of being generic and simpler, do not attempt to *understand* the workload being performed. For example Travis simply asks for commands to execute inside a VM and presents the output log as the result. Contrast that with the Drupal testbot which knows the tests being run and what they are being run against. Why is this useful? Concurrency.

Instead of running all the test cases for a single patch on one machine, the test cases for a patch may be split out into separate chunks. Each chunk is processed on a different machine and the results are returned to the system. Because the system understands the results it can reassemble the chunked results in a useful way. Instead of an endlessly growing wait time as more tests are added and instead of having nine machines sitting idle while one machine runs the entire test suite all ten can be used on every patch. The wait time effectively becomes the time required to run the slowest test case. Instead of waiting 45 minutes one would only wait perhaps 1 minute. The difference becomes more exaggerated over time as more tests are added.

In addition to the enormous improvement in turnaround time which enables the development workflow to process much faster you can now find new ways to use those machine resources. Like testing contrib projects against core commits, or compatibility tests between contrib modules, or retesting all patches on commit to related project, or checking what other patches a patch will break (to name a few). Can you even imagine? A Drupal sprint where the queue builds up an order of magnitude more slowly and runs through the queue 40x faster?

Now imagine having additional resources automatically started when the need arises. No need to imagine...it works (and did so 5 years ago). Dynamic spinning up of EC2 resources which could obviously be applied to other services that provide an API.

This single advantage and the world of possibility it makes available should be enough to justify the system, but there are plenty more items to consider which were all implemented and will not be present in the proposed initiative solution.

# Five Years Later

Five years after the original proposal, Drupal is left with a testbot that has languished and received no feature development. Contrast that with Drupal having continued to lead the way in automated testing with a system that shares many of the successful facets of travis-ci (which was developed later) and is superior in other aspects.

As was evident five years ago the testbot cannot be supported in the way much of Drupal development is funded since the testbot is not a site building component placed in a production site. This fact drove the development of a business model that could support the testbot and has proven to be accurate since the current efforts continue to be plagued by under-resourcing. One could argue the situation is even more dire since Drupal got a "freebie" so to speak with me donating nearly full-time for a couple of years versus the two spare time contributors that exist now.

On top of lack of resources the current initiative, whose stated goal is to "modernize" the testbot, is needlessly recreating the entire system instead of just adding Docker to the existing system. None of the other components being used can be described as "modern" since most pre-date the current system. Overall, this appears to be nothing more than code churn.

Assuming the code churn is completed some time far in the future; a migration plan is created, developed, and performed; and everything goes swimmingly, Drupal will have exactly what it has now. Perhaps some of the plugins already built in the ReviewDriven system will be ported and provide a few small improvements, but nothing overarching or worth the decade it took to get there. In fact the system will needlessly require a much rarer skill set, far more interactions between disparate components, and complexity to be understood just to be maintained.

Contrast that with an existing system that can run the entire test suite against a patch across a multitude of machines, seamlessly stitch the results together, and post back the result in under a minute. Contrast that with having that system in place five years ago. Contrast that with the whole slew of improvements that could have also been completed in the four years hence by a passionate, full-time team. Contrast that with at the very least deploying that system today. Does this not bother anyone else?

Contrast that with Drupal being the envy of the open source world, having deployed a solution superior to travis-ci and years earlier.

Please post feedback on [drupal.org issue](https://www.drupal.org/node/2425683).

---
created: 1317857833
layout: post
redirect_from:
- article/85/
- node/85/
- the-woes-of-the-testbot/
title: 'Part 1: The woes of the testbot'
---
The intent of this series of posts is not to blame people, but rather to point out the testbot needs full-time attention. Integral to this story are the decisions and circumstances that led me to stop working on SimpleTest in core and the "testbot" which runs on <a href="http://qa.drupal.org/">qa.drupal.org</a>. I intend to follow-up this post with others dealing with rejuvenation of the testbot and improvements to SimpleTest. I understand some will not agree with my position, but I would like everyone to understand my reasons and intentions, and how we find ourselves in the current state of affairs. After everything is out in the open, my hope is that a useful discussion will ensue and meaningful progress will result.

<h1>Factors</h1>
Four factors led me to stop working on SimpleTest in core and the testbot:
<ul>
<li>I no longer had gratuitious amounts of free time.</li>
<li>I now had a need to make a living (and working on the testbot does not generate any income).</li>
<li>The core development process being what it is led to burnout and lack of desire.</li>
<li>The request to stop working on the testbot in conjunction with the Drupal 7 code freeze.</li>
</ul>
With me out of the picture, it magnified the fact that noone else worked on the testbot and, going forward, noone stepped up to take my place.

<h1>Background</h1>
Lets start off with some background about my involvement with the Drupal testing story.

<h3>SimpleTest's journey to core</h3>
Rewind the clock back to early 2008. I had gotten involved in Drupal through GHOP and became maintainer of <a href="http://drupal.org/project/simpletest">SimpleTest</a>. I proceeded to perform a large-scale refactoring and cleaning up of SimpleTest. This, combined with other community efforts, resulted in SimpleTest being added to <a href="http://drupal.org/">Drupal</a> 7 core during the <a href="/success-at-paris-coding-sprint">Paris Coding Sprint</a>. The rapid pace at which I was able to develop SimpleTest quickly slowed as I no longer had the ability to commit changes nor make design decisions. Instead, even the most trivial changes took days or weeks to get committed. In spite of these additional challenges, I continued to diligently work on SimpleTest in core. To my dismay I discovered on multiple occasions that large changes were virtually impossible to push through the core queue, and I spent countless hours rerolling patches and refactoring code at various developers' whims. In the end, the patches simply died, but not for lack of quality or merit.

<a href="/files/simpletest_core_log.png"><img src="/files/simpletest_core_log.png" alt="SimpleTest Transition to Core Commit Log" /></a>

The chart shows 37 commits to the SimpleTest project before and after it was added to Drupal core. It is clear the pace of development slowed immediately and lessened further with time.

Changing course I focused on small changes to SimpleTest in core, but ran into similar throughput issues. For all intents and purposes, my ability to make contributions to SimpleTest had ground to a halt. This led me to write a <a href="/diaries-of-a-core-developer">blog post detailing the problem and possible solutions</a>. I was not alone in my conclusions and many would still like to see the problem resolved. I continued to contribute to core now and then, but I was completely burned out. I even took month long breaks from Drupal as it literally burned me out to try to make any contribution to core. My burnout was not caused by overwork but was due to frustration with the exaggerated length of time to accomplish a minor commit.

<h3>Following up SimpleTest with the testbot</h3>
On a parallel track, getting SimpleTest into core turned out to be only half of the battle. Actually seeing the tests adopted and maintained remained a challenge. I led the charge to keep the tests in sync (initially doing it almost alone). The effort to create an automated system for running the tests had been underway for quite some time, but lacked the necessary volunteers and commitment to really get it off the ground. I was then asked to take over the project at which point I evaluated its status and decided to start over. I created <a href="http://drupal.org/project/project_issue_file_review">PIFR</a>, a <a href="/automated-patch-testing-%28testing.drupal.org%29-realized">plan for realizing the goal</a>, and proceeded to rapidly make progress. <a href="/testing.drupal.org-is-running">Testing.drupal.org launched</a> shortly afterward and testing became an integral part of the Drupal core workflow.

With a working system I then <a href="/the-future-of-automated-patch-testing">laid</a> <a href="/testbed-design-change-and-development">plans</a> for a second iteration of the testbot with a number of improvements. After <a href="/automated-testing-system-2.0-final-steps">heavy</a> <a href="/automated-testing-system-2.0-new-features-part-1">development</a> the <a href="/drupal-automated-testing-system-2.0-launched">second generation of the testing system was launched</a> with a massively improved feature set.

<h3>Seeking sponsors</h3>
After graduation from high school I was no longer financially able to devote large portions of my time to the testing system or core development so I sought sponsors to enable me to continue my work. <a href="/acquia-internship">Acquia provided an internship</a> that allowed me to focus on testing again. After successfully completing the internship I found a job with <a href="http://examiner.com/">Examiner.com</a> that allowed me to spend a portion of my time improving and maintaining the automated testing system and <a href="/automated-testing-2.2-and-the-future">roll out the initial work for contributed project testing and a number of other improvements</a> in ATS (PIFR and PIFT) 2.2. The contributed project testing with dependencies was labeled beta because it did not support specific versions and had known issues. The plan was to make a followup release to solve the issues.

<h3>Code freeze and the request to stop</h3>
After deploying PIFR 2.2, I was asked to stop making changes to the testbot to ensure stability of the testing system during the final stages of Drupal 7 development. I continued to make improvements that I planned to deploy once the freeze was lifted, but the short freeze turned into months and more months. This delay ultimately forced me to stop development before the codebase diverged too much from the active testbot.

<a href="/files/pifr_pift_log.png"><img src="/files/pifr_pift_log.png" alt="PIFR and PIFT commit log" /></a>

The chart shows my combined commit activity for PIFR and PIFT and indicates the dramatic slowdown that occurred as a result of the freeze placed on the testbot.

During this time I was the only person who worked on the testbot in any significant capacity (or virtually at all). My availability for working on testing dwindled when my time with Examiner ended. This, combined with the stagnation forced upon the testbot, meant things simply ceased moving forward. The complete stagnation is seen in the long period of time between the <a href="http://drupal.org/node/698290">2.2 release</a> and the <a href="http://drupal.org/node/1108446">2.3 release</a> of PIFR on January 28, 2010 and March 28, 2011, respectively. <strong>During that entire period of more than a year no changes were made to the testbot. When changes were finally made, they were done merely out of necessity to accommodate the git migration.</strong>

<h3>Post-freeze undeployed features</h3>

Shortly after the 2.2 release I completed a number of improvements before things came to a stand-still. Some of the recent deployments have included functionality that I had completed, most notably:
<ul>
<li>Version control system abstraction and plugins for bazaar, cvs, git, and svn</li>
<li>Coder reviews in addition to testing</li>
<li>Beta support for contributed project testing with dependencies</li>
</ul>

<h1>Recent changes</h1>
As mentioned above, I had already abstracted the version control handling in the testbot and had four plugins (bazaar, cvs, git, and svn). Unfortunately, there were a number of assumptions that had to be made due to limitations with the <a href="drupal.org/project/project">project</a> module's VCS integration. These assumptions had to be updated for the shiny new <a href="http://drupal.org/project/versioncontrol">version control API</a>. The changes required were very minor and did not represent any feature improvements, but were simply part of the changes necessary to complete the git migration. <a href="http://drupal.org/user/30906">Randy Fay</a> made the necessary changes and the testbot saw its first update in a very long time. A few small followups were released as part of the planned phasing out of the old patch format and such. It is interesting to note the other major components of the Drupal.org migration were <a href="https://association.drupal.org/node/609">contracted by the Drupal Association</a> except the automated testing system.

<a href="http://drupal.org/user/148199">Jeremy Thorson</a> has recently been working on using the testbot's ability to perform <a href="http://drupal.org/project/coder">coder reviews</a> to help solve the woefully broken project application process which he describes in <a href="http://jthorson.doesdrupal.com/node/1">several blog posts</a>. Again we see change coming to the testbot out of necessity rather than a focused plan for improvement. For those not aware of it, the <a href="http://drupal.org/project/projectapplications">project application queue</a> has several hundred applications and it takes months to even receive a review. Jeremy has worked hard on <a href="http://jthorson.doesdrupal.com/node/28">improving the application process</a>, at the heart of which is the ability to perform automated coder reviews. Providing automated reviews has been held back on multiple fronts not the least of which is finding people to get things done. This is a definite hurdle considering that <strong>only three people have every worked on the testbot code itself not to mention there is an average of less than one active maintainer at any give time</strong>.

As mentioned above, I had deployed the first stage of contributed project testing over a year ago, but was forced to shelve the follow-up deployments. The code to properly handle module dependencies fell into disarray with the git migration and required refactoring to work with the version control API. <a href="http://drupal.org/user/46549">Derek Wright</a> and I spent a lot of time hashing out the details to ensure things were properly abstracted for the project module. I completed the code, but it was never committed and thus was not maintained through the migration. Randy took it upon himself to update the code, but deviated from the agreed upon design. This choice meant the code would not be included in the project module and has a number of other ramifications. The feature was rebuilt in a drupal.org specific manner that precludes others from taking advantage of the code and eliminates the possibility of exposing the data through the update XML information. Exposing the data in that fashion would mean projects like <a href="http://drupal.org/project/drush">drush</a>, <a href="http://drupal.org/project/drush_make">drush make</a>, <a href="http://www.aegirproject.org/">Aegir</a> and others could discard code written to recreate this data or would now be able to support proper dependency handling. In addition, the recent deployment of dependency handling has led to large delays and instability in the testbot.

<h1>Conclusion</h1>
The decision to freeze the testbot in conjunction with the Drupal 7 code freeze made sense at the time. However, the extended freeze of the testbot (due to the extended Drupal 7 code freeze) along with moving SimpleTest into core had the unintended and disappointing side effect of causing the effective stagnation of the testing system. The only changes to the testbot in the past 20 months have been made out of necessity and annoyance (the git migration and the unfinished testbot integration with the project application process for new developers). During my tenure with Examiner.com, a fair number of changes were made to the testing system but not deployed on drupal.org. The module dependency code had been written over a year ago and finalized shortly thereafter but languished and was never deployed. Recently, some of these changes were finally deployed along with the git migration. All the while, I had set forth a detailed roadmap for the testing system.

The testing system had been stable and running for 3 years. Recent changes (implemented by others) have resulted in the ups and downs of the testing system. The importance of testing to Drupal development coupled with the recent instability strongly suggests the testing system requires full-time attention. The lack of feature changes since the 2.2 release of <a href="http://drupal.org/project/project_issue_file_review">PIFR</a> in January, 2010 is a direct result of a lack of financial testing resources, the lock-down of the testing system components, the burnout caused by extreme difficulty to make changes, and the extended freeze placed on the testbot.

Various solutions were tried to enable the continuation of work on the testbot. None represented a viable long-term solution. In the end, my father and I decided the solution was to establish a business to advance testing for the Drupal community and to create an environment where we no longer have our hands tied behind our back. In the next post, I will share the vision and passion we have for testing along with several features that could be made available to the community immediately.

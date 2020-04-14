---
created: 1207429724
layout: post
redirect_from:
- article/1/
- node/1/
- wants-to-goto-paris/
title: Paris Coding Sprint - Unit Testing
---

> For posterity: http://widget.chipin.com/widget/id/51d7e4c638fa6e8b

<b>Background</b>

An overview of the plan to get SimpleTest ready for core can be found at the <a href="http://groups.drupal.org/node/10099">SimpleTest Roadmap for Drupal 7</a> post which links to the specific discussion on <a href="http://drupal.org/node/237959">unit testing</a>. Over the weeks since Drupalcon Boston 2008 we have accomplished an enormous amount of cleanup and testing. The SimpleTest module has been transformed into the future of Drupal testing.

At the sprint we intend to continue this by moving into unit testing instead of functional testing which is almost complete. The accelerated development schedule is necessary in order to meet Dries' deadline, which is fast approaching, for complete coverage of Drupal core and extend the developement cycle. Testing will not only extend the cycle of development, but also make Drupal more stable by ensuring that changes to the core do not break Drupal. The tests will provide a wall of protection as illustrated below.

Due to my role in the SimpleTest area it is important that I make it to the sprint. The work that will be completed will not just help one area of Drupal, but Drupal as a whole since it will allow faulty patches to be much more easily recognized which will make reviewing core patches much easier.

<b>Pre-Sprint</b>

Many things need to be completed before the coding sprint. The first of which is the completion of the <a href="http://drupal.org/node/237959">unit testing plan</a>. Once completed I will update <a href="http://drupal.org/project/simpletest_unit">SimpleTest Unit Testing</a> module to use the decided standards and generate the unit test stubs. The module automatically parses all core files and determines what functions need to be tested and what functions each of them are dependent on.  The stubs will then be committed to the SimpleTest repository.
<img src="/files/testWall.png" style="float: right" />
All major changes to the SimpleTest API need to be finished so that any tests written at the sprint won't have to be updated. This is almost complete, but there are still a few remaining issues.

The remaining functional tests should be completed. All the functional tests should then be reviewed to ensure that they pass on HEAD, follow coding standards, use the latest SimpleTest API, and test the necessary functionality. I have already begun this process the results of which are posted in the <a href="http://groups.drupal.org/node/9408">functional tests</a> post. By finishing functional testing it will allow all the focus to be diverted to unit testing which is key in order to get the most accomplished.

<b>Sprint Details</b>

The sprint will take place April 19<sup>th</sup> - 21<sup>st</sup>. The sprint will coincide with DrupalCamp Paris <del>in order to attract Drupal enthusiasts to help with the unit testing. Anyone interested should plan on attending the sprint at some point during their stay. There will be instructional time for those planning on contributing at the sprint</del>.

<b>What I Will Accomplish at the Sprint</b>

At the sprint I will accomplish several things in coordination with the other developers at the sprint.

<ul>
<li>Help make a significant progress in the unit tests that need to be written. Hopefully we can finish over 50% (over 960 unit tests) of the 1921 tests that need to be written for over 111 files.</li>
<li>Quickly react to any issues that arise and adjust the plan as needed.</li>
<li>Work with Karoly to ensure test creation tools for functional and unit tests are good as they can be. I will be recruiting other developers to help write tests so I need to make sure these tools are very easy to use.</li>
<li>Work with Dries to ensure unit test module I am writing provide the coverage he needs for Drupal 7.</li>
<li>Work with Kevin Bridges and Douglas Huber to make sure he can easily lead the Drupal 7 maintenance effort with the tests I am writing.  These tests may have to be updated hundreds of times, and I need to ensure we agree on how these tests should change as core changes.</li>
<li>Work with Rok to ensure the automated patch tests can run the functional tests and unit test I am writing. Help debug the tests if there are patch testing automation problems.</li>
</ul>

In order to accomplish this a group of knowledgeable developers will gather at the sprint in addition to others not coming only just for the sprint.

<ul>
<li>Karoli Negyesi</li>
<li>Rok Žlender</li>
<li>Charlie Gordon</li>
<li>Jimmy Berry</li>
<li>Kevin Bridges</li>
<li>Douglas Hubler</li>
<li>Dries Buytaert</li>
</ul>

<b>Post-Sprint</b>

After the sprint there will most likely still be some unit tests to write, but a major portion will be done and the guidelines will be affirmed. At this point developement will need to continue in order for the unit tests to be completed before the deadline.

<img src="/files/dries-timeline-drupal-7-testing.jpg" />

The community will need to rally in order to complete unit testing by the deadline only 90 days after the sprint, but the progress made at the sprint will be necessary in order for this to be plausible.

<b>My Contributions to Date</b>

<ul>
<li>I have written 9 functional tests or 27% of the functional tests once they are all completed.</li>
<li>So far I have officially reviewed 6 of the tests and will continue to review and make sure that all the tests pass and test the necessary functionality.
</li>
<li>Made 318 "commits" (as the cvs log counts them) in the past two weeks.</li>
<li>Dealt with months of backlogged issues and played a major role in the cleanup and preparation of SimpleTest for core. (over 4 pages of issues)</li><li>Spent numerous hours debugging, planning, and training others for SimpleTest.
</li>
</ul>

<b>Funding</b>

In order for this to be accomplished I need financial support. I live in the United States and will need to fly to Paris in order to contribute at the sprint. If you are in a position where you can help donate, anything helps.

You can contribute using the Chip In widget above or by clicking the following link <a href="http://www.chipin.com/contribute/id/51d7e4c638fa6e8b">Contribute (no-flash)</a>.

<b>About</b>

I am Jimmy Berry and I would like to attend the <a href="http://groups.drupal.org/node/10382">coding sprint in Paris</a> to work on unit testing for Drupal 7. I, <a href="http://drupal.org/user/214218">boombatower</a>, have become a major contributor to the <a href="http://drupal.org/project/simpletest">SimpleTest</a> module, both in <a href="http://groups.drupal.org/node/9408">functional tests</a> and as <a href="http://drupal.org/project/developers/11873">maintainer</a>. I have been working to get SimpleTest cleaned up and ready for inclusion in the Drupal 7 core.

<b>References and Additional Reading</b>

<ul>
<li><a href="http://groups.drupal.org/node/10382">http://groups.drupal.org/node/10382</a></li>
<li><a href="http://groups.drupal.org/node/9408">http://groups.drupal.org/node/9408</a></li>
<li><a href="http://groups.drupal.org/node/10099">http://groups.drupal.org/node/10099</a></li>
<li><a href="http://drupal.org/node/237959">http://drupal.org/node/237959</a></li>
<li><a href="http://drupal.org/project/issues/simpletest">http://drupal.org/project/issues/simpletest</a></li>
<li><a href="http://acquia.com/blog/kieran/how-test-20-000-drupal-7-core-patches">http://acquia.com/blog/kieran/how-test-20-000-drupal-7-core-patches</a></li>
</ul>

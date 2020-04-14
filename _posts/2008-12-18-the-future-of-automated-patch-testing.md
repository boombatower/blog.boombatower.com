---
created: 1229567487
layout: post
redirect_from:
- article/46/
- node/46/
- the-future-of-automated-patch-testing/
title: The Future of Automated Patch Testing
---
<a href="http://testing.drupal.org/node/4">Over two years of development</a> has lead to <a href="http://testing.drupal.org/">testing.drupal.org</a> becoming a reality. The testbed has been active for almost two months with virtually no issues related to the <a href="http://drupal.org/project/pifr">automated testing framework</a>. That is not to say the bot has not needed to be disabled, but instead that the issues were unrelated to the automated testing framework itself.

The stability of the framework has lead to the addition of over 6 testing servers to the network, with more in the works. Increasing the number of testing servers also means an increased load required to manage the testing network. A fair amount of labor is needed to keep the testing system running smoothly and to watch for Drupal 7 core bug introductions.

Having the testing framework in place has <a href="http://dc2009.drupalcon.org/session/saving-webchick-time-saga">saved</a> patch reviewers, specifically the Drupal 7 core maintainers: <a href="http://drupal.org/user/1">Dries</a> and <a href="http://drupal.org/user/24967">webchick</a>, countless hours that would otherwise have been spent running tests. The automated framework also ensure that tests are run against every patch before it is committed. Ensuring tests are always run has lead to a relatively stable Drupal 7 core that receives updates frequently.

Based on the overwhelming positive feedback and personal evaluation of the framework a number of improvements have been made since its deployment. The enhancements have, of course, lead to further ideas which will make the automated testing system much more powerful than it already is.

<b>Server management</b>

A number of additions have been made to make it easier to manage the testing fleet. The major issue that remains is that of confirming that a testing slave returns the proper results. The process currently requires manual testing and confirmation of the results. The process could be streamlined by adding a facility that would send a number of patches with known results to the testing slave in question. The results returned would then be compared to the expected results.

The system could be used at multiple stages during the testing process. Initially when a testing slave is added to the network it would be automatically confirmed or denied. Once confirmed the server would begin receiving patches for testing. Periodically the server would then be re-tested to ensure that it is still functioning. If not the system would automatically disable the server and notify the related server administrator.

Having this sort of functionality opens up some exciting possibilities as described bellow.

<b>Public server queue<b>

Once the automated server management framework is in place the process of adding servers to the fleet could be exposed to the public. A server donor could create an account on testing.drupal.org, enter their server information, and check back for the results of the automated testing. If errors occur the server administrator would be given common solutions in addition to the debugging facilities already provided by the system.

If the server passes inspection an administrator of testing.drupal.org would be notified. The administrator would then confirm the server donation and add the server to the fleet. Once in the fleet the server would be tested regularly like all the other servers. If the donor wishes to remove their server from the fleet they would request removal of their server on testing.drupal.org. The framework would let any tests finish running and then remove the server automatically.

A system of this kind would provide a powerful and easy way to increase the testing fleet with minimal burden to the testing.drupal.org administrators. Having a larger fleet has a number of benefits that will be discussed further.

<b>Automated core error detection</b>

Automatically testing an un-patched Drupal HEAD checkout after each commit and confirming that all tests pass would ensure that any human mistakes made during the commit process do not cause false test results. In addition to testing the core itself the detection algorithm would also disable the testing framework if drupal.org is unavailable. Currently when drupal.org goes down the testing framework continues to run which causes errors due to the patches not being accessible. Having this sort of system in place would be a great time saver for administrators and ensure that the results are always accurate.

There is currently code in place for this, but it needs expanding and testing.

<b>Multiple database testing</b>

Drupal 7 currently supports three databases and there are plans to support more. Testing patches on each of the databases is crucial to ensure that no database is neglected. Creating such as system would require a few minimal changes to the testing framework to store results related to a particular database, send a patch to be tested on each particular database, and display the results in a logical manor on drupal.org.

<b>Patch reviewing environment</b>

In addition to performing patch testing the framework could also be used to lower the barrier required to review a patch. Instead of having to apply a patch to a local development environment a reviewer would simply be required to press a button on testing.drupal.org after which he/she would be logged into an automatically setup environment with the patch applied.

This sort of system would save reviewers time and would make it much easier for non-developers to review patches, especially for usability issues.

<b>Code coverage reports</b>

Drupal 7 strives to have full test coverage. What that means is that the tests check almost every part of the Drupal core to ensure that every works as intended. It is rather difficult to gage the degree to which core is covered without the use of a code coverage reporting utility. Setting up a utility of that kind is no small task and getting results requires large amounts of CPU time.

The testing framework could be extended to automatically provide code coverage reports on a nightly basis. The reports can then be used, as they have been already, to come up with a plan for writing additional tests to fill the gaps.

<b>Performance ranking</b>

Since the tests are very CPU intensive having a good idea of the performance of a particular testing slave would be useful for ordering which servers are sent patches first. Ensuring that patches are always sent the fastest available testing server will ensure the quickest turn-around of results. The testing framework could automatically collect performance data and use an average to rank the testing server.

<b>Standard VM</b>

Creating a standard virtual machine would have a number of benefits: 1) eliminate most configuration issues, 2) provide consistent results, 3) make the processing of setting up a testing slave easier, and 4) make it possible for one testing server to test patches on different databases. Several virtual machines are currently in the works, but a standard one has yet to be agreed upon.

<b>Benefits</b>

Drupal is somewhat unique in having an automated system like the one in place. The system has already proven to be a beneficial tool and with the addition of these enhancements it will become a more integral part of Drupal development and quality assurance. Maintaining the system will be much easier, reviewing core patches will be simpler, and the testing fleet can be increased in size much more easily.

With a larger testing fleet the testing framework can be expanded to test contributed modules. In addition the framework can be modified to support testing of Drupal 6 and thus enable it to test the large number of contributed modules that have tests written. Having such a powerful tool available to contrib developers will hopefully motivate more developers to write tests for their modules and in doing so increase the stability of contributed code base.

The automated testing framework is just beginning its life-cycle and has already proven its worth, with enhancements like the ones discussed above the framework can continue to provide new tools to the Drupal community.

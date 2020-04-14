---
created: 1264715873
layout: post
redirect_from:
- article/74/
- node/74/
- automated-testing-2.2-and-the-future/
title: Automated testing 2.2 and the future
---
Most of you have probably seen the <a href="http://drupal.org">drupal.org</a> front page post concerning the <a href="http://drupal.org/node/689730">deployment of the automated testing system version 2.1</a> and the subsequent <a href="http://drupal.org/node/689990">beta phase of contrib project testing</a>. This post will provide additional details regarding the recent deployment and result of that deployment.

<b>Contributed project testing</b>
Without a doubt the biggest new feature provided by the 2.1 update was the addition of the ability to test contributed projects. The reason this took so long to support, was not due to lack of support from the testing master server or testing clients, but rather in the determination of module dependencies. In order to test contrib projects that have that have dependencies on other contrib projects the dependency relationships must be determined so the dependencies can be checked out by the testing clients.

The beta status will continue until the <a href="http://drupal.org/node/102102">dependecy parsing component</a> is complete and able to determine dependencies with 100% accuracy. I have documented what needs to be done in order to complete the code and will begin working on it, but any help would be appreciated.

The biggest issue that needs to be decided, in part by the community, is the way in which Drupal 7 style dependency information can be provided in Drupal 6 module .info files. Once we have a consensus that does not break compatibility with Drupal 6, core then the system needs to be updated to support Drupal 7.x style dependency restrictions in both 7.x and 6.x compatible projects.

On the positive side of things, we were able to track down issues with contrib testing after enabling reviews on a small number of contrib projects. With the help of the project maintainers reporting issues, I was able to fix the problems relatively quickly. Now that we have deployed the changes I have gone ahead and enabled testing on 31 projects (included Drupal core) that had <a href="http://drupal.org/node/689990">requested to be included in the beta phase</a>. It is great to see this much interest and enthusiasm by the Drupal community in embracing testing. The following is a list of the 31 projects that are currently taking advantage of the automated quality assurance system.

<div class="clear-block">
  <div style="width: 33%; float: left;">
    <ul>
      <li><a href="http://drupal.org/project/drupal">drupal</a></li>
      <li><a href="http://drupal.org/project/devel">devel</a></li>
      <li><a href="http://drupal.org/project/image">image</a></li>
      <li><a href="http://drupal.org/project/poormanscron">poormanscron</a></li>
      <li><a href="http://drupal.org/project/privatemsg">privatemsg</a></li>
      <li><a href="http://drupal.org/project/weather">weather</a></li>
      <li><a href="http://drupal.org/project/simpletest">simpletest</a></li>
      <li><a href="http://drupal.org/project/media">media</a></li>
      <li><a href="http://drupal.org/project/porterstemmer">porterstemmer</a></li>
      <li><a href="http://drupal.org/project/taxonomy_filter">taxonomy_filter</a></li>
      <li><a href="http://drupal.org/project/admin_menu">admin_menu</a></li>
    </ul>
  </div>
  <div style="width: 33%; float: left;">
    <ul>
      <li><a href="http://drupal.org/project/services">services</a></li>
      <li><a href="http://drupal.org/project/adminrole">adminrole</a></li>
      <li><a href="http://drupal.org/project/potx">potx</a></li>
      <li><a href="http://drupal.org/project/autoassignrole">autoassignrole</a></li>
      <li><a href="http://drupal.org/project/content_access">content_access</a></li>
      <li><a href="http://drupal.org/project/l10n_server">l10n_server</a></li>
      <li><a href="http://drupal.org/project/project_issue_file_test">project_issue_file_test</a></li>
      <li><a href="http://drupal.org/project/rules">rules</a></li>
      <li><a href="http://drupal.org/project/xmlsitemap">xmlsitemap</a></li>
      <li><a href="http://drupal.org/project/mollom">mollom</a></li>
    </ul>
  </div>
  <div style="width: 33%; float: left;">
    <ul>
      <li><a href="http://drupal.org/project/linkchecker">linkchecker</a></li>
      <li><a href="http://drupal.org/project/search_by_page">search_by_page</a></li>
      <li><a href="http://drupal.org/project/faces">faces</a></li>
      <li><a href="http://drupal.org/project/encrypt">encrypt</a></li>
      <li><a href="http://drupal.org/project/password_change">password_change</a></li>
      <li><a href="http://drupal.org/project/examples">examples</a></li>
      <li><a href="http://drupal.org/project/profile2">profile2</a></li>
      <li><a href="http://drupal.org/project/entity">entity</a></li>
      <li><a href="http://drupal.org/project/proxy">proxy</a></li>
      <li><a href="http://drupal.org/project/contact">contact</a></li>
    </ul>
  </div>
</div>

<b>Brief look at of ATS 2.2</b>

I wrapped a few other bug fixes along with the contrib testing bugs into a new release, which was deployed this morning. You can get a feel for what the contrib testing bugs were that we squashed in the initial testing phase below.

> PIFR 6.x-2.2, 2010-01-27
> ------------------------
> - Bugs:
>    * #695350: Provide 'last' field in pifr.retrieve() and correct query.
>    * Events should only be triggered when trigger modules is available.
>    * Remove notices when test record is saved before client record.
>    * #695278: Test list should be generated from root directory.
>    * #695278: Module 'tests' directory should be searched.
>    * #696044: Patches are not being applied properly to contrib projects.
>    * #696194: Cannot preview client test information.
>    * Do not add SimpleTest as dependency if it has already been added.

<b>Coder review</b>

In an effort to expand Drupal quality assurance beyond straight forward testing, I have written the initial integration for <a href="http://drupal.org/project/coder">coder</a> and <a href="http://drupal.org/project/coder_tough_love">coder_tough_love</a>. Since Drupal core does not currently "pass" the review I have not enabled it on the main Drupal core tests, but have instead provided a <a href="http://qa.drupal.org/head-style">stub test</a> to demonstrate the functionality and provide and public set of results to work against.

It is my goal that Drupal 7/8 core and coder/coder_tough_love will be <a href="http://drupal.org/node/666022">cleaned up</a> to the point where there are no false positives from coder and Drupal itself is clean so that the review can be enabled on all Drupal core patches and commits.

I will queue the stub test for review every few days, or upon request in IRC.

<b>Notification e-mails</b>

Another new feature provided in the 2.1 update was the addition of a trigger/actions implementation that allows e-mails to be sent from qa.drupal.org upon a test result. The system is not designed to send e-mails to specific project maintainers and such, but instead to notify test client maintainers of a client confirmation failure (client no longer working) and the core maintainers upon a bad commit that break Drupal core.

I would like to extend this system to provide general test e-mails for project maintainers and other interested parties, but that work needs to be done on the drupal.org side of things.

<b>Realizing the system's power</b>

Something that may not be apparent to everyone is that the pifr/pift system can be used for more then just reviewing patches and commits. Not only can we extend the system to perform performance analysis, test coverage analysis, textual analysis, and automatic tests/security reviews, but we can use the system as a generic platform for performing distributed operations on intelligent triggers.

For example, we could use the system as a distributed platform for generating the API documentation seen on <a href="http://api.drupal.org">api.drupal.org</a>. The system would automatically rebuild the documentation on commit and due to its distributed nature might even allow for the expansion of api.drupal.org for contrib projects (need to talk to infrastructure).

An advanced plugin API is provided for implementing additional functionality for the system. I have created an easy to extend Drupal specific implementation that should make it easy to build Drupal specific additions. Please contact me in IRC/email or the issue queue if you are interested in developing a plugin.

<b>The future and beyond</b>

I have identified items that I would like to have completed for the next installment of the system, created issues for them, and provided a summary below. I would appreciate any help from the community in completing them.

<b><a href="http://drupal.org/project/project_issue_file_review">Project Issue File Review (PIFR)</a> (qa.drupal.org & clients)</b>

<ul>
<li><a href="http://drupal.org/node/697114">Display "postponed" tests in queue graph somehow</a></li>
<li><a href="http://drupal.org/node/696434">Standardize and clarify plugin arguments</a></li>
<li><a href="http://drupal.org/node/696298">Check dependencies recursively when postponing test due to branch failure</a></li>
<li><a href="http://drupal.org/node/695400">Restrict pifr_server_test_get_since() to the project client making the request</a></li>
<li><a href="http://drupal.org/node/694876">Listing projects and current status</a></li>
<li><a href="http://drupal.org/node/679498">Change admin/pifr/server/* to admin/pifr/client/*</a></li>
<li><a href="http://drupal.org/node/679500">Change 'pifr administer' permission to 'pifr administrator'</a></li>
<li><a href="http://drupal.org/node/698402">Provide 7.x coder style checks</a></li>
<li><a href="http://drupal.org/node/698424">Provide mechanism for limiting environment (db) based on environment arguments</a></li>
</ul>

<b><a href="http://drupal.org/project/project_issue_file_test">Project Issue File Test (PIFT)</a> (drupal.org)</b>

<ul>
<li><a href="http://drupal.org/node/698396">Provide additional options for testing</a></li>
<li><a href="http://drupal.org/node/698392">Retest branches automatically on time and commit trigger</a></li>
<li><a href="http://drupal.org/node/698390">Provide e-mail notification of test results</a></li>
<li><a href="http://drupal.org/node/698386">Integrate branch tests with project page/releases</a></li>
<li><a href="http://drupal.org/node/698384">Per project branch filter</a></li>
<li><a href="http://drupal.org/node/675460">Ensure/Refactor cron_retest() query to only re-test the last file on an issue</a></li>
<li><a href="http://drupal.org/node/365517">When tests fail set Issue tag to "Failed Testing"</a></li>
</ul>

<b>Project Info (drupal.org)</b>

<ul>
<li><a href="http://drupal.org/node/102102#comment-2531920">Parse project .info files: present module list and dependency information</a></li>
</ul>

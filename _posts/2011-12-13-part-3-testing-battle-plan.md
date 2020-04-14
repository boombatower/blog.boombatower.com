---
created: 1323751913
layout: post
redirect_from:
- article/87/
- node/87/
- testing-battle-plan/
title: 'Part 3: Testing battle plan'
---
I have been meaning to make this post for quite some time. I have posted pieces before and had many discussions, but I have yet to write it out formally. Moshe's recent post about <a href="https://www.acquia.com/upal">Upal</a> motivated me to finally write the post.

<h3>Background</h3>
The SimpleTest code for mimicking browser behavior, specifically the form handling, has required a fair amount of upkeep, improvement, and generally wasted resources that could be better applied elsewhere. I attempted to clean things up with the <a href="http://drupal.org/project/browser">external browser component</a>, but that effort ended up dying. Overtime I became more and more convinced that we should revisit the basis for our testing system and try to rebuild things on an existing framework.

You may be wondering why we ever decided to create our own framework in the first place, which is indeed a good question. Back before we had a testing framework in core the concern with adding the <a href="http://simpletest.org/">SimpleTest.org</a> library to core was its size. At the time SimpleTest was not ready to depend on PHP 5 and specifically SimpleXML. It soon became clear that we could develop our own internal browser using a combination of PHP 5 tools that ended up being MUCH smaller then SimpleTest.org's implementation. This process was led by the poor assumption that we should commit the testing framework to Drupal core. In hindsight that was probably not the best idea.

Rather then commit a third-party library to core it should simply be included using a build script (like many other projects) and Drupal has a system tailored to do exactly that, <a href="http://drupal.org/project/drush_make">Drush make</a>. We could even use this approach for jQuery instead of committing the entire library into the core repository. Drupal.org already supports invoking Drush make scripts during release building so the general public wouldn't even notice the difference.

In addition to the size problem is the <a href="/the-woes-of-the-testbot">issue of bandwidth</a>, which myself and many others have discussed many times before. Keeping the Drupal testing integration (with testing library) in contrib would allow it to be maintained and more easily developed while removing an unnecessary burden from Drupal core.

<h3>Combination of tools</h3>
It is great to see Moshe's post and plan for building atop PHPUnit. Using PHPUnit definitely seems like a great start as it provides integration with many other tools, a familiar API, and using drush removes the "two Drupal sites" problem, but PHPUnit doesn't replace the browser component nor provide a JavaScript testing platform. At this point it seems prudent to build the functional tests atop Selenium which allows the tests to be written in PHP while elevating the need for the custom browser component and allowing for JavaScript testing.

I was part of the initial effort to get <a href="http://docs.jquery.com/QUnit">QUnit</a> into core along with <a href="http://drupal.org/user/157412">cwgordon7</a>. We had a working version integrated with the test runner, but things were derailed for various reasons which has since spawned the <a href="http://drupal.org/project/qunit">QUnit project</a> on Drupal.org.

<a href="http://drupal.org/node/237566#comment-5252636">Webchick summed it up</a>:
<blockquote>
Yeah, ideally we use PHPUnit and QUnit for PHP/JS unit testing, respectively, and Selenium for functional testing. Those are each the best tools for the job.
</blockquote>

Selenium has also received some love in the form of <a href="http://drupal.org/project/selenium">SimpleTest integration</a> project on Drupal.org. We have the makings of a great testing setup, but we need to put the pieces together.

<h3>Battle plan</h3>
Moving forward it seems prudent to continue maintaining each of the pieces in their respective locations and outside of core.
<ul>
<li>Selenium project needs to refactor to build on Upal</li>
<li>Rebuild DrupalWebTestCase as a compatibility layer on top of Selenium</li>
<li>Integrate QUnit project with Upal in a similiar fashion to Selenium</li>
<li>Provide PHPUnit test output parser for testbot</li>
<li>Provide a drush make script for testing in core or in a central hub repository in contrib</li>
<li>Inventory and refactor Drupal 8 tests to use new system while removing duplicity and waste</li>
</ul>

Lets make this happen!

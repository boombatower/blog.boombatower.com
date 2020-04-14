---
created: 1276291881
layout: post
redirect_from:
- article/76/
- node/76/
- drupalcon-sf-quality-assurance-thoughts/
title: Drupalcon SF - Quality assurance thoughts
---
<b>Preface</b>
Before I get to the actual body of this post I would like to give an explanation for my somewhat distant behavior the last month or so since <a href="http://sf2010.drupal.org/">Druaplcon SF</a> and the reason for this post being so long in the making. I have been going through some life changes and issues that have required most of my attention and left me with little to time for the Drupal community. I have resolved the issues that were consuming my time and I am looking forward to picking up where I left off. Hopefully, you will see a lot more activity from me in the near future.

<b>Summary</b>
We had an educational discussion during the quality assurance break-out session at the <a href="http://sf2010.drupal.org/conference/core-developer-summit">Core Developer Summit</a>. During the session we discussed the following topics.
<ul>
  <li>JavaScript testing for Drupal</li>
  <li>Site-builder testing tools</li>
  <li>Drupal core performance tests</li>
  <li>Ensuring its easy to start testing</li>
</ul>

I was charged with leading the discussion and taking notes. The following are my notes of the conversation that took place during the session.

<ul>
  <li>JavaScript testing for Drupal
  <ul>
    <li>Use <a href="http://testswarm.com">testswarm</a> to crowd source JavaScript testing.</li>
    <li>Either, test HEAD/branches only and do so against tagged versions or wait for a single browser result to come back and do on patches.</li>
    <li>Determine list of browsers/configurations we official support and that must pass JavaScript tests.</li>
    <li>Look into themes breaking JavaScript, possibly run core JavaScript tests against contributed themes.</li>
    <li>Selenium seems to have limited run-ability.</li>
  </ul>
  </li>
  <li>Site-builder testing tools
  <ul>
    <li>Provide base set of tests to ensure a Drupal server is well.</li>
    <li>Provides the ability to run tests against non-Drupal sites which can be useful when porting sites, working with sites that are not entirely written in Drupal, and for checking third-party integrations.</li>
    <li>Maintain site-builder tools in 7.x-2.x branch of SimpleTest in contrib.</li>
    <li>Possibly provide a slimmed down version of SimpleTest for use outside of Drupal.</li>
  </ul>
  </li>
  <li>Drupal core performance tests
  <ul>
    <li>Does not have to be complicated, bug simply provide a consistent benchmark.</li>
    <li>Something like loading several URLs a number of times on the same server and configuration.</li>
    <li>Have a scripted setup containing lots of content on server.</li>
    <li>Provides another use-case for extracting the SimpleTest browser for use in core and elsewhere.</li>
    <li>Simple graph of performance over time.</li>
    <li>Possible initial performance suite
    <ul>
      <li>View several anonymous pages</li>
      <li>Login</li>
      <li>Create a node</li>
      <li>Make a comment</li>
      <li>View several administration screens</li>
      <li>Load modules page (historically one of the slowest)</li>
      <li>Logout</li>
    </ul>
    </li>
  </ul>
  </li>
  <li>Ensuring its easy to start testing
  <ul>
    <li>Use <a href="http://seleniumhq.org/projects/ide/">Selenium IDE</a> combined with <a href="http://drupal.org/project/simpletest_selenium">simpletest_selenium</a> to make it easy to create basic tests.</li>
    <li>Submit native Selenium IDE output with bug reports to make it simple for developers to re-create bug and check if bug still exists.</li>
    <li>Could also be used by experienced developers to create a good basis for a test.</li>
  </ul>
  </li>
</ul>

<b>Thoughts</b>

After letting everything digest I have a number of thoughts regarding the discussion and ideas that were presented as well as a few additional pieces of information. First of all I want to share my thoughts on JavaScript testing, as I am not sure I was able to properly present this idea in person.

I look at JavaScript testing the same way I look at the current PHP based testing we do. We assume a number of things work and are tested by other organizations. As such we do not duplicate those testing efforts ourselves which is a wise decision. What I mean by that is we assume the PHP language to work as expected, the SQL language and database engine to work, and a number of other components to function. No where in our testing system do we attempt to ensure that the PHP language constructs behave as they should. We should treat <a href="http://jquery.com">jQuery</a> as the language that it is and assume, just as with PHP, that the language functions properly in all supported environments.

The implications of the above may not be clear. What the above implies is that we do not spend time ensuring that our components and JavaScript interactions function in all browsers, environments, and operating systems. Instead we leave that job to the folks at jQuery whom already do extensive testing. Drupal should focus on ensuring that the widgets/components that core provides, such as the form API autocomplete and the ctools framework function properly. This means that we can use a tool like <a href="http://simile.mit.edu/wiki/Crowbar">Crowbar</a> or <a href="http://webkit.org">Webkit</a> to interpret the JavaScript/jQuery and run our tests in that manor.

Attempting to test our JavaScript implementation on the infinite number of environments that exist adds a large amount of complexity to our work-flow and, as far as I can tell, very little gain. Unless someone can come up with a solid reason why we need to run our JavaScript tests on lots of environments I do not feel the idea is worth any more consideration. Oddly enough the proponents of it seemed willing to delude to waiting for <em>one</em> environment to return before reporting the results on drupal.org. It seems we have a lot of interest around the idea with little concern given to the implementation and cost vs benefit.

I propose we evaluate JavaScript testing frameworks with this in mind. We also need to be aware that we do not need to re-test the whole of Drupal using a JavaScript testing framework. On the contrary we need to ensure that our components and interactions work in a generic form and leave the actual testing of the final operations such as submitting a form to the already existing PHP tests. Maintaining two suites of tests that cover the same ground would be a big mistake that I hope we do not make.

<div style="float: right;">
<img src="/files/selenium-vs-drupal-testing.png" alt="Selenium vs Drupal Testing" />
</div>
To solidify this point further let's compare the popular JavaScript testing framework <a href="http://seleniumhq.org/">Selenium</a> to our current PHP testing framework. After you boil down the features and purpose of the two systems you discover that they both focus on the same key ability, that being to submit forms and perform actions as a user would. The area that Selenium allows us to test that our current system does not is in regards to JavaScript interpretation, while our current system allows us to test the PHP API directly, interact with the database, and even perform unit testing. So in order to give ourselves a fully rounded test framework we simply need to fill in the small bit that our current system does not give us.

More specifically, we need to be able to test our JavaScript behaviors and components built on top of jQuery. Something more along the lines of <a href="http://docs.jquery.com/QUnit">qUnit</a> seems appropriate since it focuses on doing just that. We will most likely develop some wrapper code for Drupal specific things, but the library provides us with a much closer starting point. There is already a <a href="http://drupal.org/node/237566">patch</a> that takes us most of the way.

<b>Plans</b>

The site-builder tools discussed will be maintained in the <a href="http://drupal.org/project/simpletest">SimpleTest</a> 7.x-2.x branch and hopefully committed to Drupal 8 once development has begun. I will continue to work on improving the tools provided to site-builders in regards to testing in the 2.x branch and will also provide back-ports of these tools to the SimpleTest 6.x-2.x branch. Since these tools are relatively new I appreciate feedback.

<ul>
  <li><a href="http://drupal.org/node/758662">Remote server testing</a></li>
  <li><a href="http://drupal.org/node/666956">Database clone testing</a></li>
</ul>

---
created: 1318544551
layout: post
redirect_from:
- article/86/
- node/86/
- breathing-new-life-into-the-testbot/
title: 'Part 2: Breathing new life into the testbot'
---
The purpose of this post is to describe the solution that, after careful consideration, seems best suited to alleviating the situation described in the <a href="/the-woes-of-the-testbot">previous post</a>. Other solutions may exist that we have not considered and that will effectively solve the problem. We are open to discussing alternatives and welcome constructive comments on our proposal. At the same time, we discourage negative comments that do not offer a positive alternative. As it is clear the current situation needs improvement, simply dismissing our proposal without offering a better alternative is not useful.

<h2>Proposal</h2>

<a href="http://reviewdriven.com/">ReviewDriven</a> (RD) is a distributed quality assurance platform built to provide a simple yet powerful interface that makes it easy to apply the best practices of continuous integration, test driven development, and automated quality reviews to your development life cycle. The ReviewDriven stack provides a completely rebuilt system designed to take advantage of Drupal 7 and contributed modules that will allow Drupal.org, Drupal development shops, and site owners to take advantage of automated quality assurance. In Drupal terms, RD is the next generation of the testbot (<a href="http://qa.drupal.org/">qa.drupal.org</a>).

We would like to see DO and other interested parties take advantage of automated QA tools. Towards that end, we propose that DO engage RD to assume the role of the testbot and provide those same services to the Drupal community.

<h3>Advantages</h3>
<a href="/files/workflow_0.png"><img src="/files/workflow_0.png" alt="" /></a>

One of the limitations of the current system and one of the primary concerns we addressed with RD is the lack of control over the testing workflow. For example, the current workflow settings apply globally instead of on a more granular basis. In contrast, the RD platform will allow Drupal.org full control to define the workflow and settings to be used with each review. <strong>The integration between the testbot (RD) and Drupal.org will continue to be maintained as an open source module</strong> which will allow anyone to contribute ideas and changes to the QA workflow on Drupal.org. Since the ReviewDriven stack provides a versioned API, the Drupal.org integration may be maintained and updated independent of RD and on it's own schedule. <strong>This approach leaves all the control and flexibility in the hands of the Drupal community and shifts the burden of the testbot to RD.</strong>

Other challenges faced by the current system were also taken into consideration when building ReviewDriven. The ReviewDriven stack is extremely flexible which in itself solves a number of the current issues and opens up a variety of new options. This opens up the possibility of reviews for things like Coder and code coverage.

The RD stack as a whole is much more maintainable since it is built on as many contributed modules as possible. This keeps the actual codebase much smaller then the current system (PIFR). Depending on contributed modules has led us to suggest and contribute a number of improvements to those modules, and to create other contributed modules. This also implies the system is easily maintainable by the Drupal community in the event we open source the code (since the majority of the code is already maintained by others). The RD architecture is analogous to any other Drupal site (sponsored by a good citizen) in that we are maintaining the code specific to our site while contributing back to the community modifications to existing modules and new features designed as generic modules.

<h3>Putting QA in context</h3>
<a href="/files/house.png"><img src="/files/house.png" alt="Using a house as illustration for building a site on Drupal" /></a>

Our proposal hinges on the fact that the testbot (and most of Drupal.org) provides a direct benefit to everyone but, just like roads and other infrastructure, the cost needs to be shared. Core and contributed modules provide a direct benefit in an indirect way by reducing the amount of and time spent writing custom code, ensuring the base system works as intended, and allowing you to leverage with confidence that code base while building your site. Core and contributed modules represent the sure foundation upon which you build your house.

There has been plenty of discussion about the important role the testing infrastructure and testing as a whole played in Drupal 7 development. The benefit of QA is also evident by the fact that a number of very large Drupal sites launched on Drupal 7 before it was officially released. The stability and dependability offered by quality assurance testing is something everyone wants.

Drupal.org is one of very few open source projects, much less projects in general, to adopt quality assurance and testing. The Drupal core development process requires new features to have tests and bug fixes to include tests. This workflow is encouraged for contributed projects and has been adopted by many of the more used projects among others. Not only does this help ensure the stability and quality of Drupal and its contributed projects, but in turn serves as a selling point and differentiator for Drupal adoption. Given that QA is both an adoption point and a vital tool for improving Drupal, does it not follow that it makes sense to provide funding for a full-time effort towards its improvement?

Just as many Linux developers are full-time employees paid to work on improving Linux, we seek to work full-time on improving the Drupal ecosystem through quality assurance. We are not the first to be funded full-time to work on Drupal or to be paid to improve Drupal.org. A perfect example is <a href="http://drupal.org/user/24967">Angie Byron (webchick)</a> who was <a href="http://webchick.net/hook-career-alter">hired by Acquia to work full time on Drupal</a>. Just as Linux was started by hobbyists and grew into a profession, so too Drupal appears to have outgown the ability to be maintained entirely by volunteers.

<h3>Funding</h3>
We see two separate areas that need funding. The first focuses on taking advantage of the ReviewDriven platform by updating the Drupal.org integration with the new testbot (RD). The second area is the ongoing fee for use of the platform (which includes infrastructure costs). RD will use the ongoing fees to improve and maintain the platform (like any other business).

Harnessing the flexibility provided by ReviewDriven will require a large overhaul of the current Drupal.org QA integration module (PIFT). We envision Drupal taking advantage of the granular settings supported by RD to provide per project, per release, per issue, and per patch settings to control the reviews made. Granular settings will ensure that the various workflows, coding standards, and environments that exist in "contrib" can be handled properly. Many projects have different requirements or adhere to different standards between their various releases. The integration with the new testbot would remain open source as Drupal.org integration and can be funded just like any other Drupal.org project.

We would also like to see a QA status advertised on each project page, possibly even some sort of ranking based on a number of quality assurance metrics. These metrics would help people select between similarly featured modules, advertise that we do QA, and help motivate developers to adopt QA. We have many other ideas for improvements and anticipate suggestions from the community.

The ongoing service also requires ongoing funding to handle infrastructure costs, feature improvements, updates for Drupal core changes, and requests from the Drupal community would ensure that things do not stagnate. We have a vision for new features that would significantly improve the Drupal ecosystem, some of which we have discussed with a few community members.

We envision either the Drupal Association or a group of businesses and other organizations with an interest in Drupal to hire RD as the logical successor to the current testbot. Our business will be to develop and maintain the testbot for use by Drupal.org and other organizations. The same approach can then be applied to other critical peices of infrastructure such as the improvement of Drupal.org and its maintenance. We would like to pioneer this effort for Drupal to further enhance the process and tools available to Drupal contributors and the community.

Further details about the specifics of the arrangement, details of the improvements, and plans for the future can be found in our formal <a href="http://reviewdriven.com/proposal/drupal-qa-proposal.pdf">proposal</a> and <a href="http://reviewdriven.com/proposal/drupal-qa-proposal-addendum.pdf">addendum</a>.

<h2>What to expect during the transition</h2>
With both the short-term upgrades and improvements to the Drupal.org integration and the ongoing RD services funded, we see the transition to RD taking shape as follows.

The first stage of the transition will require the update of Drupal.org's integration with the testbot to provide basic connectivity with ReviewDriven. Supporting RD will require a number of changes, both user-facing and behind the scenes. In addition, just using the ReviewDriven platform will enable a number of features and workflows.

Once the initial integration is ready to be tested in production, we suggest both ReviewDriven and the predecessor be run in parallel. Running both systems in parallel will provide the community with a preview of what is to come and an opportunity for feedback. Results from the two systems can be compared to provide a final round of human checks and give people time to adjust to the new system. After the completion of the parallel phase the old testbot will be deactivated and the new system will be given priority.

The second phase involves the larger changes necessary to take advantage of ReviewDriven's features and flexibility. We will start discussions and work on this phase as the initial integration stabilizes.

Some of the exciting features we will expose to DO in one or both of the stages include:

<ul>
<li>Much improved turnaround time with the ability to scale as the test suite grows</li>
<li>Code coverage reports from test runs [picture]</li>
<li>Testing of sandboxes (both core forks and modules)</li>
<li>Support for the developer application process</li>
<li>Drush make scripts for retrieving third party libraries.</li>
<li>Drush make files in lieu of parsing the project dependencies</li>
<li>Execution of arbitrary commands during various stages of the worker processing</li>
<li>Automatic enabling of issue retesting</li>
<li>Completely automated site reviews (reviews run against a configured site)</li>
<li>Reroll of a patch (using git --rebase) on one issue after a commit on another issue (for example, <a href="http://drupal.org/node/22336">this large core change</a>)</li>
<li>Display of quality metrics</li>
<li>Visible branch test results</li>
<li>Forcing a patch to run when a branch is broken (in order to fix the branch)</li>
<li>Determine the disruption to other patches that would be caused by another patch (e.g. the patch to move all core files)</li>
<li>Run Selenium tests</li>
<li>Provide separation between the testbot and the Drupal installer by writing a special script maintained by the community</li>
</ul>

![Code coverage example](/files/coverage.png)

Example of code coverage during a test run: green = executed, red = not executed, gray = ignored (or non-executable).

<h2>Conclusion</h2>
We look forward to feedback about our proposal and encourage you to voice your opinion. Please be sure to be constructive. In case its not obvious, we are extremely passionate about doing this. So let's make this happen.

<style type="text/css">
.node div.image {
  text-align: center;
  margin: 7px;
  font-size: 85%;
  line-height: 1.5;
}
</style>

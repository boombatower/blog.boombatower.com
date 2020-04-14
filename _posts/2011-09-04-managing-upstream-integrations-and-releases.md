---
created: 1315113491
layout: post
redirect_from:
- article/83/
- node/83/
- managing-third-party-integrations-and-releases/
- managing-upstream-integrations-and-releases/
title: Managing upstream integrations and releases
---
The <a href="http://drupal.org/project/awssdk">AWS SDK for PHP Drupal integration project</a>, that I maintain, provides releases that correspond to each <a href="https://github.com/amazonwebservices/aws-sdk-for-php">release made by Amazon</a>. Having releases that correspond to an upstream source is something that you see in Linux packaging systems and offers some interesting bonuses. In Linux distribution model individuals do not monitor upstream for changes, and packages are rebuilt and increment version regardless of changes to the packaging scripts themselves. Given that Drupal distributions mimic Linux distributions in many ways and even individual site builds I think the reasons behind making corresponding releases are interesting and worth consideration.

To clarify what releases corresponding to upstream releases means consider the following from the AWS SDK for PHP project page.
<blockquote>
Since Drupal.org does not allow for three-part version numbers this project follows the AWS SDK for PHP 1.x development line and the release version mapping is as follows. The mapping shows the Drupal release to the Amazon release. Please ensure you are using a Drupal module and Amazon SDK pair that match the version number mapping. For example, use the 4.1 Drupal module with the 1.4.1 Amazon SDK.

<ul>
<li>4.x -> 1.4.x</li>
<li>3.x -> 1.3.x</li>
<li>2.x -> 1.2.x</li>
</ul>
</blockquote>

Making a release whenever upstream makes a release means that some releases may end up with little or no changes to the actual Drupal module. The AWS SDK module provides a <a href="http://drupal.org/project/drush_make">Drush Make</a> file that is updated each release to point to the matching upstream release, but that is the only change for some of the releases.

Making arbitrary releases may sound foolish at first, but consider some of the advantages.
<ul>
<li>Drupal site maintainers and distributions developers can receive update notifications from the standard Drupal core update system instead of having to monitor upstream for releases. Making it easier to watch for updates encourages keeping up-to-date with upstream changes which has implied benefits.</li>
<li>Any incompatibilities with a specific version of a third-party system or upstream library do not need to be kept track of since the integration project is always used with a specific upstream release.</li>
<li>New features or configuration options from an upstream source may be added immediately to the integration module without worry about incompatibility with older releases of the upstream source. Meaning if a configuration variable changes name or a new option is added it can be exposed through a Drupal UI without worry if people are using the appropriate upstream version (since they are always intended to work with a matching release).</li>
<li>When building a site or distribution using Drush Make specific versions of upstream libraries can be easily included using the corresponding Drupal project with a proper make file. If one wants AWS SDK 2.6 simply add `projects[awssdk] = 2.6` instead of having to include the Drupal project and override any third party generic make script.</li>
<li>Handling of updates, when necessary, is much simplier since you always know the library version that was used with the previous release.</li>
</ul>

The general pattern that seems to emmerge is that upstream sources integrate better into Drupal workflow, tools, and infrastructure when matching releases are made.

It does not seem that any other (or few if they exist) third-party integrations or library integrations on drupal.org follow this workflow. Making corresponding releases may not be appropriate in all situations, but I think it warrants consideration and I am interested to hear thoughts on the subject.

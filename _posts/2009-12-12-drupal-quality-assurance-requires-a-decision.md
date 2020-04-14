---
created: 1260577540
layout: post
redirect_from:
- article/73/
- node/73/
- drupal-quality-assurance-requires-a-decision/
title: Drupal quality assurance requires a decision
---
Many of you may be familiar with my previous post <a href="/diaries-of-a-core-developer">Diaries of a Core Developer</a> and that sadly nothing has changed. We had a lot of good discussion in the comments, but as far as I understand nothing has been done to improve the development workflow.

Drupal QA has and is currently having issues due to a complex problem with the core design of SimpleTest. I <a href="http://drupal.org/node/560646">documented this problem some time ago</a>, and made several attempts to refactor SimpleTest (before and after). The patches were quite large since they did a proper overhaul of the system. I was told to break them into smaller pieces so they would be easier to review, but after discovering how intertwined the code was and not receiving any other real feedback I was burned out after a week of working on the patch (and the lack of time others were willing to spend).

Just as the database layer and other large changes had large patches and lots of follow-up patches, even SimpleTest itself was introduced in a large patch, so does this refactoring.  I <a href="http://drupal.org/node/560646#comment-2366954">briefly described</a> what needs to be done and some of the benefits in the original issue. You can find more details on the refactoring in one of my <a href="http://drupal.org/node/488182">later consolidation issues</a> in which I attempted to explain the design and work through the issue.

As I <a href="http://drupal.org/node/560646#comment-2366954">explained in the issue</a> there are some hackish ways we can work around the problem for the time being. I am happy to implement those fixes, given that I am not wasting my time. I still believe an overall refactoring is in order to fix a number of problems including proper isolation of test processes and a clean way of gather errors.

Before I commit a large amount of time, again, to refactoring the system I would like confirmation that someone will actually review the patch, those involved are fine with the changes, and that we will see this through to be committed (preferably some assurance from Dries of webchick). Of course this leads back the fact that I have no authority as a maintainer to make this crucial decision and that the core maintainers are bogged down reviewing ALL patches, instead of distributing the load to the sub-maintainers and core maintainers reviewing sub-maintainers patches and overall changes.

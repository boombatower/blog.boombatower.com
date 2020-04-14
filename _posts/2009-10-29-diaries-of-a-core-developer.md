---
created: 1256791838
layout: post
redirect_from:
- article/71/
- node/71/
- diaries-of-a-core-developer/
title: Diaries of a Core Developer
---
I am posting this in light of <a href="http://webchick.net">webchick's</a> insightful series of blog posts, <a href="http://www.mattfarina.com/2009/9/scaling-core-development-process">a more recent revival of the topic</a>, and my own utter frustration. I understand this subject has been brought up before with no change so I was a bit reluctant to bring it up again, but I think it is important. Personally I have been so fed up with core development that I have taken over a month off from serious core development. Given that other developers express this sentiment from time to time and that you can feel the discontent in the community (granted some are perfectly happy) I think things need to change.

<em>Keep in mind I have reworked this quite a bit over the course of several weeks and sat on the whole subject for some time to ensure that I had my perspective right. Not all my reasons, experiences, and thoughts are included as I tried to focus on the most important.</em>

I am going to present some of my own experiences and my personal mindset when working on Drupal core and the reasons behind it. I will not point to specific issues or people.

<b>General patch workflow/mindset</b>

In general when I submit a patch I expect there to be several revisions of the patch before it is finally committed. I think that is a healthy sign and the patch ends up better at the end. Another thing I expect is the patch to take 1-4 weeks to get committed on average with a few exceptions on both both ends of the spectrum. It usually takes a week or two to get a review and bring it to RTBC status and then anywhere from from a day to three weeks to be committed.

Given that most patches are not standalone, that is to say they are part of a larger set of changes, it can take an extremely long time to get any sort of major change into core. A shortcut to this is the controversial and feared "large patch." There are two reactions to such a patch. If the patch is a cleanup/refactoring (ie. unexciting) or for whatever whim it may be rejected as "two large to review" and the creator is told to split it into smaller patches. In many cases this is virtually impossible and if possible will take months to get them all the patches committed, not to mention the overload of work to split the patch up. I have let a number of large cleanups of the testing system die for this reason and reworking smaller patches is a joke. To accomplish any major overhaul can easily take 10 patches at ~3 weeks a piece it is impractical. Which means SimpleTest still has issues with static variables, after several attempts, among other things.

There is a more general implication of this, I do not even bother rolling several patches at a time to the same system because they will sit so long that I will be forced to waste time continually re-rolling them. I have attempted this on several occasions hoping I could get things to move a bit faster, but became utterly frustrated. Additionally, I personally ignore minor changes and cleanups, because I do not feel like going through the hassle of getting an obvious patch committed.

<b>Responsibility without authority</b>

Another problem that I have run into is architecture decisions. These decisions tend to result in massive wastes of time from stagnation or constant rewriting.  As <a href="http://www.mattfarina.com/2009/9/scaling-core-development-process#comment-2991">Larry Garfield noted</a> (excerpt):

> Schema API is right now lagging far behind the rest of the DB layer. It's API is old-school, it has limited capabilities, it's nearly useless outside of update hooks, and it's totally out of sync with the rest of the system. In April, I posted an issue laying out two different ways we could take Schema API. I asked "OK, so which do we want to do?" because as Matt says above no one wants to spend dozens of hours on one approach only to be told later "nah, let's do the other one instead". You'd charge a client extra to tell you that, but I can't bill Dries for my core development time. :-)
>
> That was in mid-April.
>
> I personally asked Dries for feedback by email.
>
> I personally asked webchick for feedback in IRC.
>
> I emailed the dev list pointing people to the issue.
>
> I got involved in a lengthy discussion on the dev list, asking "so as DB Maintainer, what the heck do I actually DO? Do I have the authority to just make this decision and not have it overruled on a whim later?"

There are always multiple ways of doing something and many times people do not comment until they see something they do not like. A problem is that in many cases an entire rewrite is required to satisfy them when many times it is just not necessary. Having more people with authority on large decisions will help alleviate this stress.

<b>Bestow responsibility</b>

Now some may say, "So what are you suggesting? Give <strike>more</strike> authority to the 'Maintainers'?" People may then look at various core developers, many of whom hold "maintainer" positions and point at the patches that they marked as RTBC that were not. I know I am guilty of this and many other developers as well. My reasoning is that a patch can be debated for centuries before it is marked RTBC and there will still be complaints made so I would rather get those complaints early on rather then later (after waiting a week+ for a review in RTBC queue of course). Better to get my patch's place in line than debate it before an initial core committer review. I can say for a fact I review my code more thoroughly for my contrib modules since I have commit access to them. I am sure the same would hold true if more core developers were given commit access.

Others may say that it is good to have patches thoroughly reviewed by people who are intimately involved with Drupal (core committers) as they will ensure the patches are better. Granted many reviews I receive in such a manor are very thorough and well thought out, but there are plenty of terrible patches that get by which I can spot flaws with. Everyone is human and no one can catch everything. Then of course I must wait weeks to get the fix in for those patches. Of course the argument above is only valid if you assume a larger commit group means that unqualified developers will review patches. If you take a look at the <a href="http://cvs.drupal.org/viewvc.py/drupal/drupal/MAINTAINERS.txt?view=markup">MAINTAINERS.txt</a> file in core I think you will find that quite the opposite is true.

Another note on the reviews. Many times a patch will improve a system or bit of code dramatically and there are further improvements planned. In the interest of incremental changes (and <a href="http://en.wikipedia.org/wiki/Extreme_Programming">Extreme Programming</a>) it makes sense to commit the code even if it is not as good as it could be. When writing a follow-up patch one will most likely find shortcomings of the previous patch and clean it up along the way. This process is much faster and much more effective. This allows the majority of code to be better overall instead of just select areas that are reviewed with intense scrutiny.

<b>Conclusion</b>

From what I can see this aged process of core development is holding back Drupal and asking for a fork. It is easily conceivable that enough people will get annoyed (especially as Drupal continues to grow) with the slow speed of development. I do not want to see that happen and I would like to see Drupal get better, faster.

I do not pretend to have all the answers, but I can present the issues I have encountered and logical responses as I see them.

Firstly, regardless of what you look at: patches sitting in RTBC queue, developers sitting for months waiting for a decision, or the fact that a "code slush" is even needed it is clear to see that two committers is not enough. Many people will argue we need more reviewers, but one must look at the RTBC queue and see that would only make things worse. Given that systems like the Schema API are not up-to-date with DBTNG, even though that was introduced very early on in D7 cycle, you know we have issues. I can say for a fact that I was just totally burned out after rewriting the internal browser patch on every whim and as such it is not implemented anywhere in core and I can only imagine that this holds true for numerous systems.

<b>Proposal</b>

I do not think that adding one, even two committers is the solution. We need a committer for each system (like every other open source project I know and Drupal contrib), who knows that system inside and out. Given that we already have a list of those people, who have no authority at the moment, the fix seems easy to implement. I do not mean that the database maintainer should only have access to /includes/database as that leads to the cases where two lines need to be changed elsewhere or perhaps all modules need to be updated for database API changes and now the maintainer is powerless. Instead maintainers should be watched over by the community and possibly 1-2 overall maintainers, like we have now.

The maintainers then have final say over their respective areas, but should take into consideration discussion with the community and of course the overall maintainers. In the situation Larry described, he would be free to decide on a path and implement it given that no feedback was provided and noone presented any valid reasons against the approach.

Defining some sort of overall roadmap, like what core is to be makes sense and that may fall more in the realm of the overall maintainer(s) and the community. The maintainers are then bound within that roadmap.

I am sure there are additional ways to improve the core development cycle, but I believe the above changes are crucial. If nothing changes, I suspect I will focus on hacking contrib after the D7 bug cycle.

<b>Thoughts</b>

I appreciate thoughts and comments on this issue, but I would appreciate it if only those "actively involved" in Drupal core development would comment as those are the individuals most effected by this.

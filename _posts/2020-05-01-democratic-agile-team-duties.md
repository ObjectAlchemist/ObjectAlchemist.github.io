---
layout: post
title: Democratic duties
description: Correct understanding of your duties in a democratic agile team
modified: 2020-05-01
tags: [SCRUM, AGILE, TEAM]
comments: false
---

Working in an agile environment gives you a lot of freedom. Your team can decide how it works, which technologies it want to use and who to welcome as a new team member. Importend descisions are made by a team referendum. And most importend, you have the freedom to choose what you like to do for the team and what not. Right?

Wrong! Or better: Not in any situation. There are duties that require to overrule your freedom.

<!--break-->

![Statue of justice]({{ site.url }}/images/Justice.jpg)

How it works in politics
----------

Lets take a look into politics first. One of the tasks we expect to solve from our politicians is to ensure our defence, in other words providing a military. Many people don't like to work as a soldier for different reasons, so I think it's a very good example. 

For this excurse we assume that a normal functioning standing army requires 10 of 10000 citizens to be a soldier. To fullfill this requirement in germany we have a professional army that pays good salaries as incentive. If there are 10/10000 or more people willing to choose the job it works perfectly fine. But what happens if this number drops below 10/10000? 9/10000 might work as well with some limitations, 8/10000 starts to be problematic. Politicians have to act now, for instance by increasing the money for the military so it's possible to increase the salaries and advertisement to find enough volunteers as result again. Puh, problem solved.

You know it, there might be more. What happens when even the double salary don't have the expected result? Maybe most of the people in the country really hate to do this job, for an unknown reason. Money is limited, therefore increasing it is also not accepted in the nation any more. The politicians now have to make a hard decision. To fullfill the task they might increase the money for the military even if the people hate them for that or changing the politics by pronouncing a compulsory military service. Surveys told them the last solution is less painful for the people, so they change the law. We can say the citizens decided it indirectly by not providing enough volunteers any more.

Will it work? Not immediately, because one piece is missing. Politicians have to ensure that every choosen will do the duty as expected. They have to add a punishment for all citizens that don't accept their duty. The combination of salary and punishment works as incentive now.

Facit
----------

As long as there are enough volunteers to do the task nobody has to do something he/she don't like. To increase the number of volunteers we need an incentive, in the example it was simply money in the first place. When having not enough volunteers anymore and increasing the positive incentives is no longer possible, you have to think about negative incentives and a new way to achive your goal again. And most importendly, the solution needs to be just for everyone! We still want to live in a democracy, right?

A similar problem in an agile team
----------

Now lets switch back to our agile team. Currently I face a similar scenario in my team that was the original reason for this article. What I think is logical and just, doesn't seem to be recognized as a democratic solution for anyone. Maybe this will change opinions or give me new insights in later discussions.

In my current iOS development team we have a 3 people subteam with enhanced access rights for administration like our CI build infrastructure. More are not allowed by the company security. Less has a high risk of possible infrastructure unavailability (e.g. one is in holiday, the other one is ill, noone can fix anything). The whole team was staffed as Swift/ObjectiveC developer, neither of us is a professional administrator or want to be one. Company security makes it also a very high effort to administer and maintain the infrastructure for non-professional administrators like us. So we came to a point that the volunteers count dropped below the required 3 persons (I'm one of the remaining 2). One of the non-volunteers also makes it very clear that he will not do that part of the job under any circumstances. How to solve that situation?

First question we have to ask ourselves "What are the incentives?". In fact it seems there are no real incentives. We (the two volunteers) do not earn additional money or any other compensation. I think my collegue volunteered because he likes to do that job, maybe he wants to proof himself for future salary class increasing, I don't know. So there might be personal incentives coming from himself, not the team. In my case it's similar. I choosed to volunteer because I don't like to damage my reputation as a high professional with a "he is not willing to do the necessary" mark. So it's also a personal incentive not coming from the team. It seems there is no simple way to "increase the positive incentives" like the salary for the soldiers, or?

Is it a duty like the compulsory military service in my example above? Maybe most of my team don't think it is, but what happens if the duty is not fullfilled? Our build infrastructure is not available/maintained and we can not provide a security proven way to build apps for Testflight, neither a release nor a hotfix. In summary the team is unable to fullfill the main task. The team has no worth for the company any more. Same would happen if the politicians are unable to solve the military problem above, the nation would be defenseless. We can't allow that! You may argue that there are already two volunteers doing the job. That's true, but if we do not have 3 volunteers we punish the two indirectly. Their holiday plans are no longer allowed to overlap. If one is in holiday and the other one becomes ill the first one might be called back from holiday in case of need. For moral reasons it's not a good idea to handle it like that. Then it will be only a question of time when the volunteer count drops to 1 or zero. So yes, in my opinion it is already a team duty and we have to act consequently.

What now? Roll a dice and problem is solved? What happens if the denier is choosen? Can you force him to do something he don't like? As one of the two volunteers I also don't like the idea to work with someone with that opinion. We need someone who is willing! How about a vote first? Every non-volunteer has to answer the question "Are you willing to do the job if the dice choose you?" in open, everyone should see the answers. The choice has a deep impact! If you choose "yes" you can't argue later that you didn't had a choice, there is no grumble about that later. The whole team saw you accepted the task. We gave you a choice, right? 

More interesting is the answer "no". Without a punishment every non-volunteer will choose it. Thats why we have to define one, and it should be a hard one or it will not work. I think someone who isn't willing to work 100% for the team should face the risk of replacement, so start looking for a new developer/administrator. Maybe the team has more money later to avoid replacement when the new one arrived, maybe not. Maybe it needs a year to find someone, maybe we find one next week. It's a risk. A potential denier choose that fate for him-/herself! As compensation he/she don't have to do that role. So spare him/her when rolling the dice now. 

Is that process just? Yes in my opinion it is. With that process it's easy to find people for any role in the team. You don't need long discussions or to beg your team for volunteers any more.

![Flowchart]({{ site.url }}/images/DemocraticAssignment.jpg)

---
layout: post
title: A pragmatic approach to letting things go
author: olivergondza
---

The things come and go. The flow of new things coming in
our lives does not need much help to sustain as it is fairly easy to get people
excited about creating things. Tools, libraries, frameworks, services - you name it.

The other side of the coin, getting rid of some of these, needs some love to prevent
the bloating that can occur when discontinuing of old things is neglected. This
is not really surprising as the value of removal is lot harder to showcase next
to creating new things, and, let's not forget it, it is just lot less fun.

## Name It

At first, whatever is being discontinued needs to be identified and named unambiguously.
This sounds trivial though it involves identifying all the purposes it could have
served to anybody. Only after we know that, we truly know what we are getting
rid of from user's perspective and what impact it can have. Needless to say, this
can get challenging easily as people driving these activities are typically not
users themselves.

## Identify stakeholders

Which bring us to the users. Environments are seldom so simple and well documented
the list of dependent parties is known and up-to-date when it is the time to kiss
it goodbye. Figuring out who the stakeholders are can require quite some
searching and asking around. In my experience, this part is most commonly underestimated
causing crucial information to be revealed way too late ("But we depend on that!").
This can get decommissioning project severely delayed or even aborted, or users
disappointed when it continues - choose what's worse. Do yourself a favor, and
identify dealbreakers as soon as you can.

## Say It Out Loud

With userbase identified and the service clearly defined, we can communicate the intentions
of stopping it. To facilitate the message reception, it is necessary to describe
the services in a way the users would understand. What's more, they need to be
able to recognize whether it is relevant to them with as little effort as possible.
Failing to make this easy on people, can cause the work put in previous two phases
gets wasted, and we are going to make more damage than necessary.

Once the word is out, do it again, and again. While repetition is useful,
using different channels to reach people is even more efficient to minimize the
chance people neglect the message being unaware it is relevant to them. The more
fuzz, the better.

Picturing the plan with timeline is advised at this place to make the expectations
clear. Though what I consider crucial is clearly communicating the way people can
communicate back any additional information or objections we have not found
ourselves.

## Break It

With reasonable confidence in our plan we gained going this far, it is about the
time to test how well we did.
Almost always there are parties we either failed to include into correspondence,
or users that did not read the message or failed to act the way we hoped they would.

In order to dry-run what would happen once the service is gone, it needs to be
broken intentionally. No matter how barbaric this sounds, it is a very pragmatic
way to ensure we are doing things safe. The breakage of a service needs to be
carefully organized in a way that it:

- Prevents all its possible uses to function, in a way a consumer would notice.
- If possible, points back to whoever is running the termination of a service so
the missed communication can be reattempted as new users or use-cases are discovered.
- But most important of all, permits the system to be reverted to functional state quickly.

Now just sit, wait, and listen carefully.

## Kill It

At last, once noone objected (in a way we would not be able to manage) to announcements
or breakage, we would have surely caused provided someone relayed on the service,
we are as confident as we are ever going to get to kill the service.

Note it can easily happen some of the feedback will cause us to change the timeplan
or the scope of the project. It is not untypical this gets delayed as users migrate
away, or until some new project is completed as alternative is yet to be created.
And that is all expected - this whole exercise was to identify what we are
confident to remove without disruption.

The important thing to understand is, none of this is an exact science. One can
always spend more time in any stage of the preparation. Despite the temptation to identify
more users or uses of the service or just spend more time waiting for the simulated
breakage to manifest, it is up to us to find a sweet spot. After all, it is
done to prevent things to hang around forever, and the same applies
to decommissioning projects as well.

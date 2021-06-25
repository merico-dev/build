June 25, 2021
# Building in Public

As a business with a new open source release, we're thinking a lot about how to transition our way of working. Business and open source are really different. The differences are not superficial.

Releasing our code wasn't a small project. Becoming open is bigger.

For example, our internal company chat generally runs on on a product called [Lark (or Feishu)](https://en.wikipedia.org/wiki/Lark_(software)). We started using it because it has translation built in and our people are very much global. That translation feature is fundamental for communicating across language barriers. But Lark is not open by any means - every seat is paid for, every user must be added by an administrator, every session requires a client-side app.

Opening up our workbench conversations is a natural part of building in public. How do we do that?

This links up with the issue of how we handle support requests, bug reports, and pull requests. From [The Four Opens](https://www.openstack.org/four-opens/)::

> 1. Open source
> 2. Open Design
> 3. Open Development
> 4. Open Community

It's not enough to open our source, we have to open our whole process!

## Iterations

Over time we have tried out a bunch of community platforms but nothing has really stuck.

1. Intercom

Our initial ideas was to have live chat tool on every page. Intercom was the natural thing because of the visual design and apparent usability. On closer inspection, though, it turned out to be oriented towards making enterprise sales.

2. Various OSS alternatives

We hunted around for open source tools similar to Intercom. There definitely are good projects out there. However, they needed developer time to install and configure, and they're still oriented towards commercial interactions. 

We made friends and learned valuable things, but in the end we moved on to…

3. A message form in [our contact page](https://merico.build/contact)

We made a Contact Us section in our product website. This forwarded the messages to an internal email list, so the first available person could reply. This worked fairly well and does get used. 

The problem is that we want our conversations to be public. The goal is to create community instead of just doing customer support.

4. Slack

Many of us have Slack open all day, every day, so it is a natural platform. However, the dynamics of a large open source community are different from a business. The limits on the free version didn't work for us, and the paid version is expensive at scale -$5 per user per month.

5. Zulip

We tried out an open source tool called [Zulip](https://zulip.com/). 

We liked the vibe of the developers and our early experiments at using the tool were positive.

Our community voted with their feet, though. People just didn't hang out there.

I think the issue was just market share: our channel was the only Zulip any of us had open, and as a result we had to start up the app or go to the site every time we wanted to connect.

6. Discord

Currently we're trying out Discord.

Most of us have it open anyway. We have accounts already. For people to join us on Discord is no harder than to joining us in Slack.

The expense is low compared to Slack. There are roles and moderation features. There is even integrated video and voice conferencing, a feature of Lark that is often helpful and that Slack charges for. 

Discord is buggy, but people seem to be able to live with that. The things we mainly miss are threaded chat and auto-translation. 

Check it out: https://discord.com/invite/83rDG6ydVZ

## So, Yeah

Maybe we'll stay in Discord, maybe not. If not the next platform I want to try out is pure GitHub, using their issues, Wiki, etc.

As we go along we can at least document our learning process. That's the reason to write this blog. 

---
June 18, 2021
# Announcing open sourcing

https://merico.substack.com/publish/post/37457083


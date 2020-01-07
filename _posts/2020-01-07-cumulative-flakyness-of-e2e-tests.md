---
tags:
- testing
- programming
layout: post
title: Cumulative flakiness of e2e suite
date: 2020-01-07 00:00:00 +0000

---
John, Ed, and Jenna were working on an exciting project. Everyone was caring about code quality and stability of the pipeline. They never merged anything without tests. John, Ed, and Jenna wanted the project to be enjoyable for a long time, and they aspired to be delivering features at a constant pace all the time.

The project code had a very high level of code coverage, it had a great CI/CD toolkit, and it was amusing to work with it.

None of the developers would ever merge any code that would have failing tests, and all the new changes merged to master pipeline, always passing all the tests. They had designed an excellent test pyramid, where unit tests are most abundant, and the e2e tests sit on top of it and verify various system integration.

John noticed a few times that during development, his e2e tests sometimes fail when he pushes new changes to his branch. But once he finished, the build usually passed green, so he was not interested in looking in those failures. They were probably not significant anyways.

It happened a few times to Ed that he had to restart his build, just before the merge, because there were one or two tests that failed. "It just sometimes works most of the time. And I'm merging green build in the end." he thought.

One day, a big opportunity arose; the project got a new, important client. Everyone got very exited and started to work, integrating new features.

"I can't get any build to pass," said Jenna. "Me neither" John and scratched his head.

"I don't understand what happened; we always merge green builds with high coverage, did anything change in the environment?" wondered Ed.

"Did you ever see it on your branches?" asked Jenna. "Yeah, sometimes," Ed blushed, realizing what they have just done. "We have accumulated flakiness in the master branch. Our little carelessness here and there drove us to a massive disaster."

"We must change that. From today we always going to verify all the failures, even if they seem unimportant," said John.

And the team got together, fixed flaky tests that were blocking new existing client, and never again ignored any failures that there were seeing during development. The new client was a massive success, and the team grew confidence. They can overcome any new challenges in the feature.
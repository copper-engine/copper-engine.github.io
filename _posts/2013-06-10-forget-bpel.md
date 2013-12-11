---
layout: blog
title: COPPER - Forget BPEL!
author: Dirk
tags: ["BPEL", "BPMN", "WTF", "Hammer and nails"]
categories: fun
---

Some myths never die. Although [BPEL](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=wsbpel) clearly is an XML dialect and does not define [nor is it meant to be a graphical representaion of processes](http://en.wikipedia.org/wiki/Business_Process_Execution_Language#BPEL_design_goals) some people *insist* of using graphical tools to model the most complex workflows, just because they *can*. This can lead to absurd monster diagrams like [this one](/images/BPEL-example-big.jpg) *(be aware: 710kb, 1866x10826px; use your browser to zoom)*. No, I'm not making this up. This is a **real world workflow** from one of our customers which is actually in use. *(Ok, I admit it has been anonymized.)*

I guess the poor developer who has the questionable honor to maintain this mess might have come to the conclusion, just after adding the 500th arrow somewhere in the lower 30 feet (when printed on fanfold paper), that there's something gone horribly wrong here. But, of course, it's not *his* decision to get rid of it; it had most likely been a *management decision* to introduce all that fancy UI stuff that looked great on glossy paper. It's the *"if-you-got-one-hammer-everything-looks-like-a-nail"* philosophy that went berserk here. We claim that it is system immanent for BPEL tools.

How much better would it have been if this workflow has been described and implemented in Java. Every developer could easily read and understand it. There's no [impedance mismatch](http://impedancemismatch.com/).

One often heard argument for graphical notations is that they bridge the gap between development team and business side. That's true. But it only works on a very basic and abstract architectural level. It's a misconception that you should use graphical notations for actual implementation, too. The problem is: once you got these fancy UI tools, no one tells you where to stop!

 Implementing a workflow is a complicated technical task where the best help is a complex *framework* that takes the burden from you of caring about all the nasty technical details of persistence, repeatability, reliability etc. and lets you concentrate on the actual business rules. That's what [COPPER](/about/) is for.

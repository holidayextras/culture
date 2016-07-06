# Technical Debt

> a concept in programming that reflects the extra development work that arises when code that is easy to implement in the short run is used instead of applying the best overall solution

Tech Debt is an inevitable part of programming, a valuable tool even. Do not shy away from it. Embrace it! Subdue it! Make it your servant. Whatever you do, *do not just ignore it* and hope it goes away.

*Note:* This document is aspirational, it doesn't necessarily reflect the current culture of technical debt at Holiday Extras. It does however reflect where we would like to be, and is a work in progress. If you have ideas about how to better manage technical debt please tell people. Good places to do this are the #developers, #guid-front-end and #guild-back-end slack channels or with a PR for this page.

## The Seven Commandments of Tech Debt

### 1. Be Courageous! Push Back When Appropriate.

This commandment comes from our general company values. All our values are relevant however this one particularly so.

*Be courageous* and push back against pressure to ship more and more technical debt without considerations for the long term implications. Be respectful of other stakeholders but make sure that you speak up when you think the issue of technical debt is not being addressed correctly.

### 2. Anticipate Tech Debt From the Beginning

Tech Debt to some extent is part and parcel of every piece of code that you write. Before you lay down a single line of code, you should be thinking about tech debt. Perhaps even as soon as you are aware of upcoming work. It is never too early to be thinking about:

* existing debt that might slow you down
* existing debt that it makes sense to pay off
* potential new debt that it makes sense to accrue
* how you might be able to avoid adding more tech debt

### 3. Let's Talk About Tech Debt

Once you have some picture of the above you should be constantly communicating your thoughts on tech debt to others:

* your pod lead
  * tech debt impacts cost and cost impact priorities
  * any decision to accrue tech debt *has to* accept the long term cost
* developers/software engineers
  * you may have insights into tech debt that will impact their work (and they yours)
  * you may be able to help them avoid tech debt in the first place (and they you)
* software architects
  * they have some of the best knowledge on existing tech debt
  * they will be able to advise how to avoid tech debt in the first place
  * they can help make your case to others that a certain piece of tech debt needs to be addressed
  * you can help keep them up to date with the general picture of tech debt

#### Where to do this?

The existing communication channels are perfect for talking about tech debt. We just need to make sure we are using them.

* *Stand ups* can be a great place to flag any technical debt you are facing or expecting to face in the immediate term.
* *Sprint planning* can be used to flag any technical debt that may impact upcoming work, and can be used to plan work to address existing debt.
* *Retrospectives* are ideal for reflecting on whether technical debt had unforeseen consequences, the overall state of tech debt and whether we need to make any changes to deal with it.
* *Work in Progress* pull requests can be used to get feedback on changes before they are ready for formal code review. *Early feedback can go a long way to avoid technical debt in the first place.*
* *Pull requests* are perfect for talking about tech debt in the context of a specific change. It is the PR owner's job to draw reviewers' attention to these considerations, and reviewers should ensure that PR owners are doing this correctly. PRs should outline:
  * what existing technical debt the changes are impacted by
  * what, if anything, the changes do to help address existing tech debt
  * what technical debt the changes introduce or compound, and any justification for them
  * what GitHub issues and/or Jiras have been raised to address any new debt in the future
* *Guilds* can be used to see if people in other pods are having a similar experience with an existing piece of tech debt.

### 4. Limit its influence

Technical debt that is isolated has much less scope to interfere with other code. This tends to emerge naturally if code is loosely coupled and modular in nature.

Say you have a module and its internal implementation is mess, but its interface is well thought out and does not change very often. This is an example of isolated technical debt. The messy internals will rarely interfere with work on the wider application. If however the interface becomes the subject of many changes, or it has a lot of bugs, it might be a good time to refactor or even just throw it out and start again.

### 5. Be deliberate

Whilst it is "okay" to take on technical debt, it has to be a deliberate decision. The short term gains of shipping it now have to be weighed against the long term interest that will need to be paid off over its lifetime, not to mention the debt itself.

These decisions have to consider the business demands and the technical implications. They must be made by pod leads and developers in concert. By their nature it is impossible for either to do it on their own.

It is the responsibility of developers to explain to leads the current state of technical debt and any implications it will have on current or future work. It is the responsibility of leads to explain to developers the priorities of the business and the urgency and importance of work.

When it comes to paying off existing debt, focus on debt that currently has or is likely to have in the near future a "high rate of interest". Technical debt that is isolated can be safely left for another day.

### 6. Little and often

Technical debt should be paid of continuously in small payments. Exactly how much is up for debate. But here are some things to bare in mind:

* Little and often over big and rarely.
* Big payoffs may be required from time to time.
* Avoid big refactors that drag on for more than one sprint, aka "Refactor Purgatory".
* If a big rewrite is required do everything you can to transition to a new design incrementally.
* Even if a sprint solely addresses tech debt it should be split into iterative chunks if possible.

### 7. Aspire to be a boy scout

> Always leave the campground cleaner than you found it.

All other things being equal a change to the code base should leave it in a better state than you found it. In reality this might not be possible. Business demands may require things to go out without any improvements. Sometimes the only improvement you can make to code would be so comprehensive it risks confusing the purpose of the PR and justifies its own.

That said it is a good mind set to carry with you. At the very least you should be making notes for yourself and your "troop" about specific things that you need to come back and clean up later.

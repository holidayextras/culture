# Pairing

Pair programming is the process of two people sharing a development environment to solve a technical problem. Outside of programming, pairing can be a used to solve all manner of diverse problems.

## When should we pair?

There's no concrete right or wrong time, but it's worth keeping in mind that the way you pair needs modifying to make sure you're achieving that goal. At HX, we would typically use pairing to onboard new starters, solve difficult problems and spread knowledge.

The Scrum Alliance describe the pair as a Driver, in control of the development environment and a Navigator providing instruction. This slightly over simplifies the relationship, so we'll break out some of the different ways we pair depending on the situation.

### Onboarding

During onboarding you would typically pair a new starter with an experienced engineer. The intention is for the experienced engineer to provide the context and safety net needed for the new starter to learn on a real platform. It has been proven, externally and internally, to be a really effective, supportive way of getting someone up to speed.

This is the prime example where Driver and Navigator is an incomplete analogy. If the new starter is following instruction and doesn't understand *why* they're performing actions, they're unlikely to be able to complete a similar task independently in the future, which should be a key aim of pairing for onboarding. In pairing for onboarding the Navigator should more resemble a Coach.

Consider following the instruction of a Sat Nav. You've been told to turn left, turn right and keep on going and reached your destination. How likely would you be to be able to repeat the journey again? Would you be able to adjust the journey if you hit a problem? You would most likley need the Sat Nav again.

Now imagine you had a local in the car who was providing context and maybe giving you a little test along the way. Instead of saying "turn left", they might say "you'll need to turn left after this pub, which has a great garden by the way" and you may ask "and if I overshoot the turning", and they may say "don't worry you can take the next left as they join up". You hopefully get the drift, and would agree you'd be much more likely to be able to repeat the journey.

An effective onboarding pair will typically have the new starter at the keyboard with the experienced engineer providing a mix of instruction and questioning. It's this questioning, and opportunity for the new starter to question their own understanding, that helps the new starter to build the context around the instruction. There is some [theory](https://en.wikipedia.org/wiki/Action_learning#Revans.27_formula) that supports this style of learning.

### Problem solving

Pairing to problem solve is essentially the modern embodiment of the 500 year old proverb that two heads are better than one. In pairing you're combining your collective experience and cognitive capacity to try and increase the likelihood and/or speed of solving a problem.

To use the Scrum Alliance analogy, the Driver and the Navigator will likely switch roles throughout the process. This is important so while one person may be typing, both are contributing significantly to the thought process.

There should still be an open, questioning dialogue between the pair. Thinking out loud will help the pair to combine their understanding of the problem and the proposed solution.

### Knowledge sharing

The more critical a part of a system the more we need to ensure we have the right solution, it's well built and we have resilience in our support of it. We would not want to find a critical part of a system has been built using an inconsistent strategy, in a confusing way, by one person who is on holiday. Especially on the day it's broken. So, we should identify these parts and use pairing as a way to mitigate against this risk.

The approach to this pairing will be very similar to when pairing to problem solve. What we may find is that the solution is simple, but this mustn't stop us from pairing. The value is in knowing why the solution has been built that way and how it works.

## The costs of pairing

There is a great paper on the costs and benefits of pair programming here: http://collaboration.csc.ncsu.edu/laurie/Papers/XPSardinia.PDF

The study was pairing two experienced programmers. In short, the investigation found that the time take to complete a task was not doubled by working in a pair, with an average of only 15% increase in combined time spent.

The benefits included 15% fewer defects, significantly less (more concise) code and a more enjoyable experience for the pair. The long term benefit of knowledge share and better decision making are harder to prove, but for a 15% cost, the benefits are clear and worthwhile.

## How should we pair?

As a business that promotes a flexible working culture which may involve colleagues in our core office and remotely, we need to consider the best tools to use for the situation. In the office this may be screen mirroring so both Driver and Navigator have an unimpeded view, or remotely using a collaborative screensharing tool such as Screenhero allowing both parties to contribute.

We shouldn't limit ourselves to just pairing at the computer, sketching out ideas on a whiteboard - physical or digital - together can be just as valuable, for example.

Remote working and pairing are not mutually exclusive. In fact, there has been a suggestion recently that pairing over Screenhero can be more effective than pairing on the same physical machine, as it provided greater focus and more conversation between the pair.

## Pairing with a tight deadline

Sometimes we will be pairing when we need to deliver work for a tight deadline. Often the pairing process of having a driver and navigator is not ideal for this as it is slightly slower and not as time efficient as having a developer with the required knowledge write the code themselves. In cases such as this we have seen success in having the driver completely lead the code writing whilst explaining exactly what they are doing and why, so that the navigator/navigators can still gain knowledge and provide input where they can.

Making the pairing process dynamic in situations such as this can still allow deadlines to be met, with code written well, and knowledge being shared successfully. Taking this approach also caters for having mulitple navigators, allowing knowledge to be shared to more than one other developer at a time.

## What to do next

- Give it a go and agree what you want to achieve through pairing before the session.
- At the end of the session, reflect as a pair and give each other feedback.
- See you if you can be observed by an independent third party to get some external feedback.

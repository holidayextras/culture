# Documentation For Success

_Having great documentation unlocks knowledge and breaks down silos at Holiday Extras. It is important as it enables us to be flexible and agile with our structure and approach, supporting changing teams and building better products._ 

## What is our goal?

We want to create a culture whereby our documentation defines what we are doing and have done and most importantly WHY we’ve done it. We should be able to share this with new starters and people moving teams to enable them to understand systems, teams, architectures and processes before they even meet the people they will be working with.

Our documentation will serve as a record of what we have achieved within our teams and how we did it. This will enable us to make better decisions in the future by providing data of actions taken in the past.

## Why write documentation?

Documentation serves many purposes, the three main concerns for us are:

*   onboarding and knowledge sharing
*   debugging and issue resolution
*   reference for future decisions

### Onboarding and knowledge sharing

At Holiday Extras we have historically received feedback from people onboarding into teams on the lack of documentation. This lack of documentation causes them to have to ask lots of questions and therefore increases the level of support an onboarder requires within a team. Inversely, feedback often given to new starters is around new starters asking questions and learning systems. By writing good documentation we can resolve both these types of feedback.

In having quality documentation with good coverage of our systems we can resolve the basic questions around installation, project purpose and design. By removing these questions - providing the answers proactively through documentation - new team members can get to the important questions more quickly and allow them to have an impact on team delivery more quickly.

### Debugging and issue resolution

Documentation can provide support when debugging a problem. Having an easily digestible and up to date resource which explains our services can simplify the process of identifying where a problem originates, how it differs from expected behaviour as well as where to begin with resolving it. All at a high level without having to read code. At the most basic level good documentation should highlight the purpose and objectives of a service or platform. Just having this information can aid you in excluding sources during issue investigation and resolution.

### Reference for future decisions

Furthermore, documentation can provide an archive of decisions made within teams. It outlines the architecture and implementation details of a project explaining why these decisions we made. By writing the documentation as you design you create a contract within the team as to how a project is going to look and what it is and isn’t expected to do and be.

Returning to a project you haven’t worked on for a period of time can require a re-onboarding period before you are able to make any progress. Whether this is in the urgency of a production incident or as a part of regular sprint work having good quality documentation will enable you to understand a project easier and ultimately take action sooner.

## Our current progress

Within Holiday Extras we have metrics which highlight the level of documentation within each repository. Each project repository in GitHub is owned by a team and it is their responsibility to maintain the documentation therein. Following the introduction of metrics around this documentation we have seen an improvement in our documentation coverage, but we think we can do more!

We document our teams and processes through the use of Google Sites. Our roadmaps are shared through a team owned [site](https://sites.google.com/holidayextras.com/journey/home) which allows for collaboration and general enquiry into the direction of our products and platforms. Our processes are documented in much the same way with Google Sites around learning and development through to agile processes.

## How do we get there?

There are some really simple changes we can make to the way that we work to ensure that documentation remains up to date and isn’t a chore to write. When you have to do bulk writes and updates it takes time. We should instead be focussing on a little and often approach, the same approach we take when writing features.

### Treat documentation as a first class citizen

Documentation is part of your solution, whether that solution is technical, process driven or other. Ideally the documentation is the first part of the solution you create. When you come up with an idea or you’re looking at a ticket and deciding how to approach it – this is your opportunity to create documentation. By doing this everyone involved in a project will have the same understanding of what is going to be achieved and how. When doing technical planning meetings, backlog reviews or just having discussions within your team make sure the output is recorded somewhere centralised and available for all to see.

As best practices here are a few pointers to follow:

**Documentation should be written before any code is committed.**
As your documentation is your design you should be confident in this before you start shipping any changes. This may be through discussions held on JIRA, designs mocked up on a whiteboard and then shared in a doc, a TPM document, a README or a good description on a PR. Identify within your teams the best place and best practices for recording these decisions.

For small changes and quick fixes this could just be a well defined JIRA and a descriptive PR. For example in an expedite when attempting to quickly debug an issue, having this level of documentation greatly increases the speed at which we can discard changes as a cause.

For larger projects such as integrations with suppliers the documentation might be a TPM doc, a README on the repo which describes the integration and the purpose and a write up in a shared drive of the business rules surrounding a project. This will help technical people understand how the integration is implemented and anyone non-technical understand how and why we work with the supplier.

There are multiple levels at which documentation can be written and we can’t be overly prescriptive on defining exactly is required for each instance. It requires a level of pragmatism when deciding what to record and how. A few things to consider might be how likely the thing you’re documenting is to change, how long are we going to be investing in it and how complex or different to the norm is it. These are just a few examples of the questions you can ask yourself, always look to balance the investment spent with the benefit gained and remember that we can always do more retrospectively if required.

**Review existing documentation when making changes.** 
When you write code you’re changing how a system works, so check the documentation first to see what might need updating and do so within your PRs or as a part of your definition of done.

**PR documentation like you would code.** 
The documentation is there as instruction to everyone, so it needs to be understandable by everyone. When you’re making a change to documentation get someone to proofread it for you. When you’re reviewing the change make sure you read it fully and don’t skim over it. Will this make sense to you in six months time when you’ve forgotten the context? Be accountable and own this!

### Share what documentation you have

Some teams are already doing a great job at documenting their processes, decision and code. The next step is to make sure that the documentation that is written is shared and used within the team. If you’ve got someone joining your team, point them in the direction of your documentation; if someone is asking you a question, do you have documentation that explains the answer? If not, should you create it?

Also you should feel empowered to feedback to others if you rely on something and the documentation is not up to scratch. For example, if you’re in an expedite and a change isn’t well documented, or the documentation is out of date, which slows down the solution you should feel confident in feeding back to the team or person responsible. This will help to support a better expedite process in the future.

Having good documentation is only half the battle. We need to make sure that we’re using it effectively by **linking and referring to it**.

## When is it enough?

A common question around documentation is about understanding when you have enough. We can always push for more documentation, but should we? If we document down the implementation of a project to the minutest details, whenever anything changes, we have lots of documentation to maintain and update. This becomes a chore and we may miss references which we later incorrectly rely on.

When thinking about what to document and the level at which you’re documenting be thinking of the “why”. What decisions were made which led to the process, implementation or architecture that you’ve adopted? Specific implementation details such as database structures etc can always be inferred, instead invest your time describing why these decisions were made.

## Summary

What we’d like to see is more people taking action documenting their teams, processes and tickets in the future and in retrospect for work already shipped. Remember that as a member of your team it’s important to include time for documentation writing within your estimates for work. 

If we all chip in with documentation we can help set everyone up for success.

## Links for HXers

*   [dockyard metrics](https://services-hub.dock-yard.io/)
*   [team roadmaps](https://sites.google.com/holidayextras.com/journey/home)
*   [learning and development hub](https://sites.google.com/holidayextras.com/tech-team-ld-hub)
*   [agile processes hub ](https://sites.google.com/holidayextras.com/hx-agile-coaches/home)

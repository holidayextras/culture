# Expedited Procedure

Don't panic! Sometimes things do go wrong. It's important that these issues are fixed promptly, without compromising on quality. The process below will help guide you through how to identify whether an issue should be deemed 'expedited', and importantly how to proceed with providing a fix.

## Issue defined

Confirmed with a metric such as:

    X Bugsnags being generated
    Call centre reported issues
    Error screen showing raised booking errors

## Ownership

Generally the issue will be reported to web department or the someone in web will find the issue.

* First web contact takes ownership.
* The web contact follows issue through to resolution or hands over to another team member to fulfill this role.
* Use online channels to keep all interested parties updated **regularly**. See the communication section below for more info.

The owner should not also be working on resolving the issue. If this is the case, ownership should be transferred. This is to ensure that communications continue without distracting from resolution.

## Classification

If the web contact is unsure whether the issue should be expedited seek advice from a product owner before continuing.

Consider what metrics are being affected, eg:

* Conversion
* Customer experience
* Revenue

As well as the life span the of the issue.

## Team formed

Cross functional with the following elements:

* Developers to assist fix.
* Developers to be available to review code promptly.
* Tester.
* Product owner.

For out of hours use the [web contact details](https://holidayextras.jira.com/wiki/display/WEB/Web+Contact+Details) for contact info.

## Communication

### Slack

Regular communication is essential. By sending regular updates the Web Team and HX stakeholders are able to manage and mitigate the impact of the issue. It's important that attention is not drawn away from fixing the immediate issue, so consider delegating the communication to someone else if you're involved with the fix itself.

* Post in the main [#expedite](https://holidayextras.slack.com/messages/expedite/) slack channel. #expedite should be used for owner updates only, so encourage questions to be sent direct. Include when you'll next update in each post. 
* If issue is complex enough consider a dedicated Slack channel (regular communication in the primary #expedite channel should still continue).
* If the issue is going to effect deployments, post in the [#deployments](https://holidayextras.slack.com/messages/deployments/) Slack channel.

### Email

There should be three key email communications sent during an expedite. 

* An incident report email to let affected teams/individuals know of the issue.
* An email to confirm the end of the incident, with brief notes on the solution and ongoing issues.
* An email after the post-mortem to include a full summary of the incident, solution and any learning outcomes.

## Fix

Generally:

* Code developed using our best practices and procedures.

Do we need to break any of the above, if so why?

* Speed of fix
* Complexity of fix
* Lack of resources (e.g. out of hours, no-one in office)
* Temporary fix (with permanent fix following later)

**Do not** use the reasons above as a get out clause to not follow processes. Going against the process will need to be discussed at the time and justified later in your communication about the issue to the team.

## Deployment

* Code deployed in the usual manner, to allow for rollback if needed.
* Monitor original problem metric to ensure fix is successful.

## Conclusion

* Summary email to web (Include any relevant PR's/JIRA's/links).
* Communication to origin of report; call centre or other department.
* The web contact that took ownership of the issue should schedule (or delegate to another to schedule) a [blameless postmortem](Blameless-Postmortems.md).

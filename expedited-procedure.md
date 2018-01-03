# Expedited Procedure

Don't panic! Sometimes things do go wrong. It's important that these issues are fixed promptly, without compromising on quality. The process below will help guide you through how to identify whether an issue should be deemed 'expedited', and importantly how to proceed with providing a fix.

## Issue defined

Confirmed with a metric such as:

    X Bugsnags being generated
    Call centre reported issues
    Error screen showing raised booking errors

## Ownership

Generally the issue will be reported to the Web Team or the someone in Web will find the issue.

* First Web contact starts the expedite process in Slack channel.
* The Web contact begins investigating the expedite with support from an engineer with the required expertise (if not themselves) to diagnose the issue. 
* The Web contact must also find someone independent of the investigation / fix (if not themselves) who can communicate the progress and resolution of the expedite. This ensures that communication continues without impeding the progress of the expedite.
* The owner follows the expedite through to resolution or hands over to another team member to fulfill this role.
* The owner uses online channels (Slack, hangouts and email) to keep all interested parties updated **regularly**. See the "Communication" section below for more info.
* The owner speaks face to face to key internal teams, such as the Shift Manager, to gather more information on cause or symptoms of the expedite and provide feedback and reassurance that the issue is in hand. This includes providing an estimated timescale for a resolution (where possible) to help with planning any mitigating measures. For example, where the issue causes a spike in call volume a decision can be taken to add more people to the phones.



## Classification

If the Web contact is unsure whether the issue should be expedited seek advice from a Pod Lead before continuing.

Consider what metrics are being affected, eg:

* Conversion
* Customer impact
* Revenue/business impact
* Downtime or life span of the issue

## Team formed

Cross functional with the following elements:

* Owner independent from investigation and resolution, to focus exclusively on communicating progress.
* Engineers to assist fix.
* Engineers to be available to review code promptly.
* Tester

For out of hours use the [web contact details](https://docs.google.com/spreadsheets/d/1mYaqoZzzDJI_RcTtlYTpzMM85bV_2AbwUsQSGPa6Jxs/edit) for contact info.

## Communication

### Slack

Regular communication is essential. By sending regular updates the Web Team and HX stakeholders are able to manage and mitigate the impact of the issue. 

* Post in the main [#expedite](https://holidayextras.slack.com/messages/expedite/) Slack channel. This should trigger the bot and will ask you to confirm this is a new expedite.
![](/images/expedited-procedure/new_expedite.png)

* You will then be asked if you are the owner of the expedite. When confirming, this will create a new expedite channel with the current date to move all conversation away from the main channel.
![](/images/expedited-procedure/owning_expedite.png)

* #expedite should be used for owner updates only, so encourage questions to be sent directly to the temporary dated channel. 
* #expedite should have the customer impact clearly communicated and regularly updated to allow for wider business information.
* Owner to include when you'll next update in each post in #expedite.
* If the issue is going to affect deployments, post in the [#deployments](https://holidayextras.slack.com/messages/deployments/) Slack channel.

Note that Germany and the Shift Manager may not be on Slack, so you should open a separate communcation channel with them where appropriate e.g. face to face (preference) or hangouts.

### Email

There should be two key email communications sent during an expedite. 

* An incident report email to let affected teams/individuals know of the issue.
* An email to confirm the end of the incident, with brief notes on the solution and ongoing issues.
	* What happened technically? 
	* What was the customer impact? 
	* What was the resolution?
	* What's next?

Email recipients should include those involved and [this list](https://docs.google.com/a/holidayextras.com/spreadsheets/d/1I0dkaNqEv7_eQ-eRYLZ5cKY51HLz6x2wWCxYokmgps8/edit?usp=sharing)

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

* Summary email to Web (Include any relevant PR's/JIRA's/links).
* Communication to origin of report; call centre or other department.
* The owner should schedule (or delegate to another to schedule) a [Wash-Up](https://github.com/holidayextras/culture/blob/master/Blameless-Postmortems.md), which will include next steps (including any Jiras to be raised to clear any Tech Debt, for example).
* An email should be sent after the Wash-Up to include a full summary of the incident, solution and any learning outcomes.


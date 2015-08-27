# Bugsnag Process

## tl;dr
- check Bugsnag regularly
- check metrics in the morning and during the day
- send an email at the end of the day with info about the above

---------------

The Bugsnag duties rotate from developer to developer on a weekly basis.

Bugsnag is an important part of our developer role here. It allows us time to catch and resolve bugs and issues which we would not otherwise see or prioritise. This creates a better development environment for us all as well as improving the customers experience on our site. 

No one likes a buggy website, so why should we build one?

During your Bugsnag week you are removed from your usual sprint activities and instead perform the following tasks to maintain the health of our sites. If there are no other Bugsnag duties for you to perform you may continue with sprint work for that day.

The key activities you should perform are:

- checking Bugsnag for bugs being thrown
- checking HXMetrics for spikes in error rates
- sending an email out at the end of every day to update people on the above

There is also a [Bugsnag Kanban board](https://holidayextras.jira.com/secure/RapidBoard.jspa?rapidView=284) which has tasks related to known bugs which you should attempt to work through in order to reduce our overall bug count.

The most important part of Bugsnag is communication. You should be talking to people about what you're seeing and updating on things you're working on etc. Don't suffer in silence on a tough bug, ask for help. And don't assume an issue is being looked at, check!

## Bugsnag

You'll require our Bugsnag credentials. Ask the last person on bugsnag for these.

### How do I find bugs?
Errors on our site are reported to [Bugsnag](https://bugsnag.com/holiday-extras/trip-app-js/errors) and are visible in the dashboard.

The errors are split by project and you can switch between these using the menu in the top left corner.

![](/images/bugsnag-process/project-selected.png)

Errors can be viewed in either a grouped or ungrouped state.

![](/images/bugsnag-process/grouping.png)

Most people prefer the ungrouped view as the grouping isn't always accurate.

#### Grouped View
When viewing grouped errors you can however get an idea of the bugs importance.

![](/images/bugsnag-process/dashboard-bug-view.png)

The stats show you:
- occurrences (how many times has the happened)
- users (how many individual people has it happened to)
- last 2 weeks (instance graph of last two weeks)
- stage (what environment is this happening in)
- severity (how important do we think this is).

You can use the search form on the left hand side to limit what bugs you see:

![](/images/bugsnag-process/bug-filter.png)

#### Ungrouped View
Ungrouped view shows you a live error by error view of the project. It also shows you more information regarding each bug such as user and device settings.

As with the grouped view you can click on a bug to view more information.

#### Bug detail view
To get more info on a bug you click on it from the dashboard view. This takes you to more information on that specific bug. 

You can use the stats on this screen (such as the event count, user count, metrics and history) to figure out if the bug is affecting a lot of our users.

![](/images/bugsnag-process/stats.png)

You can use the information available in the tabs to find the cause and attempt to replicate the issue.

![](/images/bugsnag-process/tabs.png)

_The information and tabs available will change depending on the bug type and project._

#### Slack
One of the simplest ways to track bugs as they are happening is to subscribe to the Slack private group "bugs". If you are not in this channel then ask someone to invite you.

This channel shows you the bugs as they happen and gives links direct to their detail.

### What do I do if I find a bug?
Any bug worth mentioning is worth raising as a JIRA. There's a handy button on the Bugsnag detail view that will do this for you:

![](/images/bugsnag/create-jira.png)

Add as much detail about the bug as you can. E.g. how can you duplicate it? Who is it affecting? If it's something which should be expedited then let people know and get working on it.

### Cleaning up
At the end of your week on Bugsnag duty you should audit the bugs on the Bugsnag dashboard. Any that have not been reported within the previous two weeks should be deleted. This prevents us from getting into a state whereby we have a large and unmanageable backlog of bugs which are no longer occurring. 

If the Bugsnag issue you are removing also has a JIRA attached you should make a note on the JIRA that the bug has been closed as it is no longer occurring and close the ticket.

## hxMetrics

The [metrics system](https://metrics.holidayextras.com/) is a graph based tool which allows us to monitor the error rates on our systems. You will have to be on our WIFI or Junos to access this.

Clicking in the search bar shows all the available systems. You should mostly be concerned about production systems in your Bugsnag duties. Checking "HTTP responses" and "controllers" are the main focus points for HAPI and Render production. That doesn't mean don't check the others, just that these are the main concern.

### HTTP responses
Clicking on "HTTP responses" for a system shows you a live view of the HTTP responses that aren't good. These are listed in their count order, summarised at the top and then divided among the different urls below. 

You can change the duration and times of search at the top in the navigation bar.

![](/images/bugsnag-process/timeframe.png)

Generally speaking, the higher a number is above the "error" and "error rate" graphs - the worse things are.

Be advised though, a lot of these errors will be from user error. Use common sense when looking at an error to figure out if it's a problem with the systems/our code or a user that can't type their card number out properly.

### controllers
This shows errors at a controller and method level. The metrics displayed are the same as above, but the grouping is different.

## The EMAIL
The purpose of the email is to allow everyone to keep up to date with what went wrong during the day, why, and how it was fixed. You should include the following headings:

*Metrics*
_Tell us about any issues you saw in metrics. What was the impact? How was it resolved?_

*Bugsnag*
_Tell us about Bugsnags you've raised (with JIRA links). What is the impact? How could it be resolved?_
_Tell us about Bugsnags you've worked on (with JIRA links). What is the impact? What is the progress? Any blockers? What has your investigation found?_

*Anything Else*
_Tell us about anything else that happened during the day that may have caused issues for the customer. What sites/services went down etc? What was the impact? How was it resolved?_

The email is designed to be blameless and to allow people to learn from mistakes and understand the solutions to problems that may occur again in the future. They should be geared around the impact on the customer with metrics where available. We're not looking to blame or snoop on what you've been doing all day, but we do want to know what progress has happened, what went wrong and why.

## FAQ
Q. I've found a bug, is it important?

A. It's mostly down to stats and your gut feeling. Is it affecting people and preventing bookings? If in doubt, ask! It's better that we catch a bug that is causing problems then assume it's not an issue and let it slide.


Q. Everything is blowing up! What do I do?

A. In event of nuclear war, run and don't look back. If major bugs are happening (not the Starship Troopers kind) then let people know and get involved with fixing, check the last deploy and get them involved. Should you roll back?


Q. I've got a Bugsnag PR in review but I'm not on Bugsnag any more. What should I do?

A. It's okay to hand over work to the next person on Bugsnag. Just before you do ask yourself what the importance of this PR is? Should you be owning this into production yourself and will you feel better if you do? If you're going to loose sleep over it then I suggest you baby-sit it into live. There's no problem in doing this and if your work is good then it shouldn't take too long anyway!


Q. I have a question regarding Bugsnag, what should I do?

A. Ask someone. Then write your question and answer in here so that other people can learn. The chances are if you're thinking it, then someone else will be as well. Be courageous and ask the question no one else does. There are no stupid questions.


Q. Bugsy?

A. Don't ask.

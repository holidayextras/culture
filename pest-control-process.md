# Pest Control Process

This is a monitoring process owned by Engineers to ensure we catch and resolve bugs which slip through our automated and manual testing nets. This creates a better development environment for us all as well as improving customer experience on our site.

No one likes a buggy application, so why should we build one?

## tl;dr
- Process owned by Engineers through a working group
- Facilitated by Janitorial stretch holders 
  - Each Janitor will 'own' and oversee Pest Control for one system
- One Engineer responsible for monitoring on a daily basis
- Group meetings once a sprint
- Monthly working group meetings to review the process.

## Process

Every Engineer who has at least 6-month experience in the business is included in the Pest Control rotation, they are then responsible for monitoring systems for uncaught bugs for 1 week for around an hour per day.

Not every Engineer will have the extensive knowledge needed to triage bugs in every system, so our Janitorial stretch holders make themselves available to facilitate this triage where necessary.

This means communication is important. Engineers on rotation should be talking to people about what they're seeing and  working on. Don't suffer in silence on a tough bug, ask for help. And don't assume an issue is being looked at, check!

* Engineer rotated on a weekly basis.
  * Spend ~1 hour a day on Pest Control, but still continue with sprint work.
  * Time spent will vary, but should not be influenced by pod/sprint deadlines.
  * Keep an eye on reporting systems and logs e.g. Bugsnag, Metrics & Sumo Logic. 
    * When a new bug is caught
      * If you can recreate it, raise a `WEB` Jira with type set to `Bug`.
      * Add the `pestcontrol` label.
      * Giving details of the customer impact and how to recreate.
      * If you are unsure, consult with a Janitorial stretch holder for that system.

## Delegation meeting

The last three Engineers on rotation should attend.

* Occurs before start of sprints.
* Chaired by the Engineer currently on rotation.
* Re-triage current bugs, to ensure they can be recreated.
* Move bug Jiras to relevant pod backlogs.

## Working Group

Janitorial stretch holders and any interested Engineers are encouraged to attend to contribute to improving this process.

* Occurs every 4 weeks.
* What’s happened since last time?
  * Monitor progress of bug completion.
  * Recurring themes.
* Review current process.
* Send email update.

## Systems used

### Bugsnag

You'll require our Bugsnag credentials. Ask the last person on rotation for these.

#### How do I find bugs?
Errors on our site are reported to [Bugsnag](https://bugsnag.com/holiday-extras/trip-app-js/errors) and are visible in the dashboard. Each bugsnag that occurs is also logged into our **#bugs** channel on slack.

The errors are split by project and you can switch between these using the menu in the top left corner.

![](/images/pest-control-process/project-selector.png)

##### Inbox View
When viewing the dashboard errors in inbox you can get an idea of the bugs' importance.

![](/images/pest-control-process/dashboard-bug-view.png)

The stats show you:
- events (how many times has the happened)
- users (how many individual people has it happened to)
- last 2 weeks (instance graph of last two weeks)
- severity (how important do we think this is).

You can use the filters on the left hand side to quickly limit what bugs you see, the **introduced today** filter is particularly helpful for the pest control process.

![](/images/pest-control-process/bug-filter.png)

You can also use the search bar for more complex filtering, allowing you to search by **error message**, **release stage**, **browser version** and various other things.

_:information_source: Tip: searching by error message can be useful for working out the number of users impacted as Bugsnag's grouping isn't the most reliable and often leaves the same errors ungrouped._

![](/images/pest-control-process/bug-search.png)

##### Bug detail view
To get more info on a bug you click on it from the dashboard view. This takes you to more information on that specific bug.

You can use the stats on this screen, either in the summary on the left or the tabs along the top, to gather as much information as possible in order to assess the overall impact of a bug.

![](/images/pest-control-process/stats.png)

You can use the information available in the tabs to find the cause and attempt to replicate the issue.

![](/images/pest-control-process/tabs.png)

_The information and tabs available will change depending on the bug type and project._

#### What do I do if I find a bug?
Any bug worth mentioning is worth raising as a JIRA. There's a handy button on the Bugsnag detail view that will do this for you:

![](/images/pest-control-process/create-jira.png)

Add as much detail about the bug as you can. E.g. how can you duplicate it? Who is it affecting? If it's something which should be expedited then let people know and get working on it.

### hxMetrics

The [metrics system](https://metrics.holidayextras.com/) is a graph based tool which allows us to monitor the error rates on our systems. You will have to be on our WIFI or Junos to access this.

Clicking in the search bar shows all the available systems. You should mostly be concerned about production systems in your Bugsnag duties. Checking "HTTP responses" and "controllers" are the main focus points for HAPI and Render production. That doesn't mean don't check the others, just that these are the main concern.

#### HTTP responses
Clicking on "HTTP responses" for a system shows you a live view of the HTTP responses that aren't good. These are listed in their count order, summarised at the top and then divided among the different urls below.

You can change the duration and times of search at the top in the navigation bar.

![](/images/pest-control-process/timeframe.png)

Generally speaking, the higher a number is above the "error" and "error rate" graphs - the worse things are.

Be advised though, a lot of these errors will be from user error. Use common sense when looking at an error to figure out if it's a problem with the systems/our code or a user that can't type their card number out properly.

#### controllers
This shows errors at a controller and method level. The metrics displayed are the same as above, but the grouping is different.

### FAQ
Q. I've found a bug, is it important?

A. It's mostly down to stats and your gut feeling. Is it affecting people and preventing bookings? If in doubt, ask! It's better that we catch a bug that is causing problems then assume it's not an issue and let it slide.

Q. I have a question regarding pest control, what should I do?

A. Write your question on the working group agenda and attend the meeting. The chances are if you're thinking it, then someone else will be as well. Be courageous and ask the question no one else does. There are no stupid questions.

Q. Bugsy?

A. He's not around any more, the cute stuffed toy is now known as `the pest`.

Q. Hey, what's this new JIRA label `pestcontrol`?

A. We use this to report on how the found JIRAs are progressing please don't remove the label.

Q. Someone put a JIRA in my pod queue and it does not belong there, what do I do?

A. Our delegation process takes into consideration current pod work and also number of JIRA already given to a pod that week, you are welcome move the JIRA to another pod, please discuss this with the other POs / SMs.

Q. When do I need to complete pest control JIRAs?

A. We do not expect these JIRAs to be done in the next sprint, but as long as they don't get closed or forgotten about for months then thats fine.

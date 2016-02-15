#### What are the links to relevant tickets?
#### What does this Split Test do?
#### What variations does this split test have?
#### How should a developer review this?
#### Any background context you want to provide?
#### Screenshots (if appropriate)
---
#### Quality checklist (for PR author to complete BEFORE code review):
- [ ] I've created an experiment in Optimizely.
- [ ] I've created a split testing partial/template file (IF APPLICABLE).
- [ ] I've created a custom firing lookup table rule in GTM.
- [ ] I've checked that this is the only split test running on the page.
- [ ] I've created a new route for this split test. (ONLY APPLICABLE IF RUNNING A FULL PAGE TEST)
- [ ] I've checked this work against the requirements of the Jira.
- [ ] This change passes all necessary tests (cross browser, automated or otherwise).
- [ ] I have set up a reporting Jira to check and publish GTM work.
- [ ] I am ready for this to be code reviewed, merged and tested.

#### Reviewer 1
- [ ] I agree with the assumptions made in the quality checklist.
- [ ] I’ve witnessed the work behaving as expected.
- [ ] I’ve checked for appropriate test coverage.
- [ ] I've checked for the experiment ID in the template and for other experiments on the page.
- [ ] I’ve checked for coding anti-patterns.
- [ ] I've checked this work against the requirements of the Jira.
- [ ] I've checked that this template follows the naming structure outlined in our procedure documentation surrounding split testing.
- [ ] I don’t believe this work introduces unnecessary technical debt.
- [ ] +1 from me!

#### Reviewer 2
- [ ] I agree with the assumptions made in the quality checklist.
- [ ] I’ve witnessed the work behaving as expected.
- [ ] I’ve checked for appropriate test coverage.
- [ ] I've checked for the experiment ID in the template and for other experiments on the page.
- [ ] I’ve checked for coding anti-patterns.
- [ ] I've checked this work against the requirements of the Jira.
- [ ] I've checked that this template follows the naming structure outlined in our procedure documentation surrounding split testing.
- [ ] I don’t believe this work introduces unnecessary technical debt.
- [ ] +1 from me!
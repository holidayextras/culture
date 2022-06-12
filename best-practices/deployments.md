# Deployment Guidelines

Detailed below are general guidelines for production deployments:

- Deploy little and often, don't let work get held up in staging/integration environments.
- We should have the confidence to deploy at any time (our high unit test and Selenium coverage should back this point up).
- When running a deploy, both a **tester**, and an **engineer** should be present. This is to provide support to both parties, and puts us in a position to roll-back the changes quickly and effectively if needed.
- Own your merge - whether you are a tester or engineer, see your deployment all the way through to production.
- Deployments on non-customer facing systems (e.g. HAPI) do not explicitly require a tester to be present. However if the change has the potential to impact the customer facing systems, then this should certainly be considered.
- Before deploying, ensure you communicate what you are doing:
  - Make sure all deployments to all environments on all repos are communicated in the #deployments channel in Slack.
  - Try to communicate what you are merging as opposed to "I am merging to staging".
  - If your code will remain in a particular environment overnight, communicate with the team when you plan to merge.
  - If your plan is not communicated and blocks someones else's work, your work may be reverted.
- If the project that you are working on is versioned, you should always increment the version according to the guidelines set out in [http://semver.org/](http://semver.org/).

Obviously the above is a rough guide and _shouldn't_ replace common sense. e.g. If something happens out of hours and a developer can't get hold of a tester, they shouldn't hold off deploying a fix.

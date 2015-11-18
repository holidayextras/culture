# Deployment Guidelines

##### Detailed below are general guidelines for production deployments.

1. Deploy little and often, don't let work get held up in staging/integration environments. 
2. We should have the confidence to deploy at any time (our high unit test and Selenium coverage should back this point up).
3. When running a deploy both a **tester**, and a **developer** should be present. This is to provide support to both parties, and puts us in a position to roll-back the changes quickly and effectively if needed.
4. Own your merge - whether you are a tester or developer, see your deployment all the way through to production.
5. Deployments on non-customer facing systems (e.g. HAPI) do not explicitly require a tester to be present. However if the change has the potential to impact the customer facing systems, then this should certainly been considered.
6. Before deploying, ensure you communicate what you are doing:
  - Make sure all deployments to all environments on all repos are communicated in the Deployments chat. 
  - Try to communicate what you are merging as opposed to "I am merging to staging".
  - If your code will remain in a particular environment overnight, communicate with the team when you plan to merge.
  - If your plan is not communicated and blocks someones else's work, your work may be reverted. 
7. When making a change, you should always increment the version according to the guidelines set out in [http://semver.org/](http://semver.org/).

**Obviously the above is a rough guide and *shouldn't* replace common sense. e.g. If something happens out of hours and a developer can't get hold of a tester, they shouldn't hold off deploying a fix.**

N.B. For expedited issues please refer to the following [guide](expedited-procedure.md).

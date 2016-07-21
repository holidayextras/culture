# PR Best Practices

* Include screenshots in your pull request whenever possible to help people visualize what your trying to achieve _(Picture paints a thousand words)_.
* Include any jiras that are covering this work.
* Add issues that are addressed by this PR by linking them in the description.

### Front end changes need SEO

There are many docs in the [SEO culture folder](README.md) to help everyone to understand some of the principles and best practices we use.  
The best way to make sure is to just approach the SEO team and ask and we will help.  
When pushing changes to live that may affect the front end pages of our core sites then these will require being checked by the SEO team to make sure they adhere to the best practices we have developed over the many years.  
Any work pushed live that has not been checked could result in a whole domain being dropped from the Search engine index and a loss of millions of pounds so if in doubt, best to check.

### Try to keep them small

Try to keep your pull requests as small as possible.  
If the work your asked to do has individual components that don't require being together to function then perhaps create separate branches and PRs to get those merged.

Smaller PRs are easier to review and easier to keep up to date with master/edge branches. There's no hard rule for this, obviously changing the same thing in a hundred files is a simple thing to review, and sometimes changes just won't be simple, but we should try and make them as easy to review as possible.

### Push fast simple changes

As SEO has to move fast and innovate quickly on design and functional iterations we require the PR process on small simple changes to be speedy and readily available so here are some changes we will follow:

* Whenever changes are made that are done purely for Search engines or SEO benefits, these can be reviewed by members within the SEO team.
* Work set out at the sprint planning are and will continue to be discussed with members of the testing team to determine whether the task requires testing resource or are devqa, PRs will be dealt in the same manor.
* Work involving large functionality or key structural changes will require being reviewed by normal [PR best practices](../pr-best-practices.md)

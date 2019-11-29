# PR Best Practices

- Include screenshots and animated GIFs in your pull request whenever possible.
- Include thoughtfully-worded, well-structured specs. See our [testing standard](testing-standard.md) document.

## Keep them small

Keep your pull requests as small as possible. We should be looking for ways to split up our work into the smallest units that are sensible to merge. Some things we should be thinking:

- _"Are there any small code refactors I can do as a preliminary PR into edge that will make completing this feature a trivial task?"_
- _"Are there sub-features of this Jira ticket that can be deployed in isolation and have benefit to the product before the rest of my work is done?"_
- _"Perhaps I should merge lots of small pull requests into a separate branch for my feature and have those reviewed individually, and then merge that into edge once it's tested."_

Smaller PRs are easier to review and easier to keep up to date with master/edge branches. There's no hard rule for this, obviously changing the same thing in a hundred files is a simple thing to review, and sometimes changes just won't be simple, but we should try and make them as easy to review as possible.

## Landing page changes need SEO

There are many docs in the [SEO culture folder](../seo/README.md) to help everyone to understand some of the principles and best practices used in SEO.
The best way to make sure is to just approach the an SEO member and ask for their help.
When pushing changes to live that may affect the landing pages of our core sites then these will require being checked by the the SEO department to make sure they adhere to the best practices we have developed over the many years.
Any work pushed live that has not been checked could result in a whole domain being dropped from the Search engine index and a loss of millions of pounds so if in doubt, best to check.

## When to push SEO changes

As SEO has to move fast and innovate quickly on design and functional iterations we require the PR process on small simple changes to be speedy and readily available, also whenever changes are made that are done purely for Search engines or SEO benefits, these must be reviewed by a member within the SEO department.

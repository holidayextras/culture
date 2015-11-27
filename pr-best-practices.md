# PR Best Practices

* Include screenshots and animated GIFs in your pull request whenever possible.
* Include thoughtfully-worded, well-structured specs. See our [testing standard](testing-standard.md) document.

## Keep them small

Keep your pull requests as small as possible. We should be looking for ways to split up our work into the smallest units that are sensible to merge. Some things we should be thinking:

- *"Are there any small code refactors I can do as a preliminary PR into edge that will make completing this feature a trivial task?"*
- *"Are there sub-features of this Jira ticket that can be deployed in isolation and have benefit to the product before the rest of my work is done?"*
- *"Perhaps I should merge lots of small pull requests into a separate branch for my feature and have those reviewed individually, and then merge that into edge once it's tested."*

Smaller PRs are easier to review and easier to keep up to date with master/edge branches. There's no hard rule for this, obviously changing the same thing in a hundred files is a simple thing to review, and sometimes changes just won't be simple, but we should try and make them as easy to review as possible.
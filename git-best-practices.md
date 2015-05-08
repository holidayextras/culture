# Git Best Practices


## Branching

### The master or default branch
We should never have code in the master branch that hasn't been through code review. Therefore work should never be directly committed to this branch.

### Creating branches

Branches should be named with the applicable ticket number. This has the following benefits:
* Easy to find ticket based on the work & vice versa
* Links with Jira

**Bad:**
```
$ git checkout -b some-new-work
```

**Good:**
```
$ git checkout -b FOO-123
```

### Deleting branches
Once a branch has been merged into master, it's no longer of any use and should be deleted.

**Tip:** Remote branches can be deleted from the cli as follows
```
$ git branch -D my-branch
$ git push <remote> :my-branch
```


## Commit messages

Commit messages are a great tool for tracking changes down in git history - as long as they're written in a useful way. The more information the better, but we should be aiming for at least a sentence per message.

A commit message should always include:
* Ticket number
* Brief summary of changes

**Bad:**
```
$ git commit -m "stuff"
```

**Bad:**
```
$ git commit -m "addressing PR comments"
```

**Good:**
```
$ git commit -m "FOO-123 remove unused bar in baz view"
```

**Even better:**

Multi line commits are a great way to add more detailed messages
```
$ git commit -m "FOO-123
deleted foo.bar
moved logic from template to presenter
something else i did"
```

### Squashing

Squashing commits helps us keep out git history clean and easy to follow. We should avoid pull requests being submitted with a large amount of commits where possible. Sometimes multiple commits make sense, but not for example if they're just trying to fix tests or address PR comments. To achieve this there are a few things to keep in mind whilst working on your branch:
  * It's a lot easier to squash commits if you use rebase in your workflow instead of merge. _Don't try to rebase/squash if you've already merged, it causes pain_.
  * If someone has branched off of your branch(:trollface:), you'll cause them pain by squashing so don't.
  * This [guide](http://gitready.com/advanced/2009/02/10/squashing-commits-with-rebase.html) is useful for getting to grips with rebasing/squashing.

## Git ignore

We can use the `.gitignore` file to exclude certain files and directories from entering the repo. We should have one `.gitignore` in root, rather than multiple across all directories.

We only need to commit the source code for our applications. There are a bunch of things that sometimes find their way into git that ideally shouldn't be there.
* Modules from package managers (e.g. `node_modules/`)
* IDE project files (e.g. `foo.sublime-project`)
* Generated source from task runners/compilers

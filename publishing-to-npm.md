# Publishing to npm

For any problems that are not specific to the business, we should look to use publicly available modules and release our own when no suitable existing options are available. Many of our projects are javascript, so these modules will probably live on [npm](http://npmjs.com/).

The guidance below covers the absolute basics, some modules may automate this process and if they do should provide details of how to release them in their own README/documentation.

### Who to publish as

It's much easier to manage access through adding and removing owners than through resetting the password on an account shared by many people, so you should __publish as yourself__. Create an account with the npm website and `npm login` as that account via the CLI.

### Creating a new module

NPM's docs cover the basics of [creating](https://docs.npmjs.com/getting-started/creating-node-modules) and [publishing](https://docs.npmjs.com/getting-started/publishing-npm-packages) modules. Once your module is in a usable state, you should `npm publish`, then add the [@holidayextras](https://www.npmjs.com/~holidayextras) user as an owner (`npm owner add holidayextras module-name`) - we recommend this to ensure every module we release is under the ownership of at least two people in the organisation.

### Updating actively used modules

Release new versions as other projects require changes made to the module; you don't have to release a new version after every pull request is merged.

Speak to someone who previously published the module, or someone with access to the [@holidayextras](https://www.npmjs.com/~holidayextras) user to get yourself added as a collaborator. Follow [semver](https://docs.npmjs.com/getting-started/semantic-versioning) principles when deciding the new version number to avoid surprising any consumers of the module, and `npm publish`.

### Guidance on inactive modules

If a public module was released in the past but for whatever reason we no longer have a use for it, passing it on to a new maintainer outside the organisation is an option. If there are no obvious candidates (think people who have submitted pull requests or issues and may still need the module themselves), make it clear in the module's README that it's looking for a new owner to manage the expectations of prospective users.

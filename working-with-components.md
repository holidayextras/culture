# Working with components

## What are components
Components can be thought of as sections of user interface on our websites. There are small components, such as buttons and inputs, and larger components such as product tiles and search headers. Typically larger components will be _composed_ of many smaller components, but anything can be a component really. Designing many small components to compose together for larger reuseable components within a page is known as **atomic design**. To read more on Atomic Design see [Brad Frost's excellent post](http://bradfrost.com/blog/post/atomic-web-design/), but for the purposes of working with components at Hx we'll be referring to three of the 5 atomic design component levels:

 - **Atoms:** These are the basic building blocks of our pages. Atoms are our most simple components, such as a form label, a text input or a button.
 - **Molecules:** These are groups of atoms bonded together and form larger reuseable components. These molecules take on their own emergent properties and can in turn be reused to build even more complex sections of a user interface. An example of a molecule component could be a label and input together.
 - **Organisms:** These are groups of molecules joined together to form a relatively complex, distinct sections of our user interface. An example of an organism component may be a product tile, or a search header.

At Holiday Extras we currently use Facebook's [React](https://facebook.github.io/react/index.html) for building new composeable user interface components, but these same atomic design principles apply equally well to other templating languages we use elsewhere - haml, handlebars, etc. 

## Why should we use components?
Building a library of reuseable components has several benefits:

 - Once built, the shared components can be reused in a fraction of the time they took to build.
 - Shared components provide a consistent look and feel to all of Holiday Extras' user interfaces.
 - Shared components allow user interface improvements to be rapidly rolled out across our websites and all of our customers and partners to benefit in a fraction of the time they otherwise would.
 - Simple reuseable components can be rapidly composed together to build more complex user interfaces.
 - Reuseable components vastly reduce code duplication in the front end.

In the short term there's an obvious implemntation cost to setting all this up, but taking a long term view and keeping the above in mind, the question is really *"Why wouldn't we use components?"*.

## Where and when do we use components?
In short, we'll use them everywhere. Our goal is to be integrating reuseable components across all of our user interfaces. From our landing pages and search engines, to our web and mobile apps. In order to do this we'd like everyone to look at componentising user interfaces as they work on them.

As for when to use components, the answer to this question can be broken down into two sections:

### Writing new user interface code
This should be componentised from the beginning, and this is the expectation for new pull requests. The UX/UI Team are on hand to offer advice and assistance with this if necessary.

### Modifying existing user interface code
When modifying existing user interface code, existing user interface elements that can be replaced by shared components we already have should be replaced. If there are no existing user interface components we should look at building some when time allows.


## Where do we put shared React components?
Going back to the atomic design principles from the "What are components?" section we'd like to put our shared React components in the following places:

 - **Atom and Molecule React Components:** These go in the public [UI-Toolkit](https://github.com/holidayextras/ui-toolkit) project where they can be reused everywhere. This is because at the granular atomic and molecular level, components have applicability across all of the Hx platforms _as well as_ to the wider world. An easy comparison to make is to the Twitter Bootstrap project, it was originally built by Twitter, but was designed for everyone to use. In the same way, the UI-Toolkit project and everything that goes into it is open source.
 - **Organism React Components:** For now, keep these in the repository of the project you're working on. Very shortly we'll be setting up a repository for all of our "organism" react components, at which point we can move components and their associated tests across into a shared area.

If you're building a new React component and you're unsure about where it should go, talk to one of the SAs or the UXUI team.

## Where can I read more?

 - **React**
  - [React Docs](http://facebook.github.io/react/)
  - [UI Toolkit](http://hungrygeek.holidayextras.co.uk/ui-toolkit/)
  - [Additional documentation on technical resources](https://github.com/holidayextras/culture/blob/master/technical-resources.md)
 - **React & Tripapp**
  - [Integrating React Components Docs](https://github.com/holidayextras/tripapplite/blob/master/doc/WORKING_WITH_TRIPAPP.md#24-react-components)

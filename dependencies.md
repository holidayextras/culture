#### Dependencies

During our recent work to reduce the footprint of the tripapp JavaScript we have come across an issue that we keep bumping into which is causing our JavaScript bundle to keep increasing in size.

Many of our application have both internal and external code dependencies and these internal dependencies usually contain a set of their own dependencies. What we are finding is that when there are shared dependencies between the parent project and the child there are occasions when dependencies are updated in one project and not another.

#### Example

A perfect example of this is lodash. Lodash is used in many of our projects. If versions are not kept in sync then you can see something like this:

When versions are matched:

[Small footprint](/images/dependencies/small.png)

After I have updated the lodash version within our client application but not it's dependencies:

[Larger footprint](/images/dependencies/small.png)

The total bundle size has increased by 6.3%

When we go to generate the minified JavaScript asset it finds a version mismatch and will usually bundle both versions of the packages into the released asset which will impact the size and performance of our application.

With this in mind if you are updating dependencies in any way then it would be beneficial to check that any applications that are dependent on these libraries have their versions kept in sync.

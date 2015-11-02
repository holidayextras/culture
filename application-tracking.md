# Application Tracking

## Principles

### Why?

We build applications to support the business and drive revenue.

To associate the performance of applications and the behaviour of customers with commercial performance and revenue, we instrument our applications with Event Tracking.

Events form sequences of activity (clicked X, viewed Y), which can be correlated with outcomes (made a booking, added an upgrade).
Event sequences can be separated into different streams, or segments (by specific user, by product, by version of the application). This sequences form the foundation of business monitoring.

**Tracking is a fundamental communication mechanism between application development and commercial performance.**

We should not be asking why we should be tracking an event in an application, we should be tracking events by default.

### What?

Applications should have tracking enabled for all features that a user can interact with. When new features are written adding tracking should be a requirement for the definition of done.

In addition to the previous point extra explicit tracking should also be performed when additional information needs to be gathered. Examples of these are as follows:

 * Adding an item to a basket - Product info required.
 * Booking success - Product, upgrade and booking info required.

Tracking can also be performed on elements that are displayed to the user but are not interacted with, such as different advertising banners etc.

Events containing sensitive information such as user or card details should still be tracked but with the sensitive data removed or masked.

### How?

At an application level we should not be concerned with how data is tracked beyond calling our tracking module with the correct parameters, once this has been done we can trust that the tracking module will do its job. Currently we have more than one tracking system and have plans to change how these are implemented. We also plan to deprecate some of these tracking solutions. Abstracting the decision away from an application level allows changes to our current systems and implementing new systems to be much easier.

### Who?

Everyone in the business should have easy access to application tracking results and reports, to allow for quick educated choices when performing tasks such as prioritising work, assessing severity of bugs or deciding whether to consider legacy users.

## Current situation

* Not all events have tracking.
* Different events are tracked using different systems in a single application.
* Different applications have varying levels of tracking.
* Different areas of the business prefer to use different systems for reporting.
* Tracking data is created via code and by accessing an element's HTML5 data attributes.

## Current goal

* One single tracking module to provide an agnostic API for all our application tracking needs.
* All our applications / modules that require tracking will use the same tracking module.
* Our current tracking systems will sit behind this thin layer.
* Decide on tracking event data schema, so we are not bound to one or more current systems.
* Create schema change process involving communication and planning between engineers and data team ahead of implementation.
* Keep specific application conventions and / or technology out of the tracking module.
* Tracked elements are not accessed directly by the tracking module.
* Each tracking event will be sent to all current systems (actual data may vary, based on system).
* Different reporting systems can still be used.

## Next steps

* Review current tracking systems in use.
* Potentially change how the preferred tracking systems are fired from the tracking module.
* Review how tracking data enters the data pipeline.

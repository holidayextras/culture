# Application Tracking

## Principles

### Why?

We should not be asking why we should be tracking an event in an application, we should be tracking events by default. Events containing sensitive information such as user or card details should still be tracked but with the sensitive data removed or masked.

### What?

Applications should have tracking enabled for all features that a user can interact with. When new features are written adding tracking should be a requirement for the definition of done.

In addition to the previous point extra explicit tracking should also be performed when additional information needs to be gathered. Examples of these are as follows:

 * Adding an item to a basket - Product info required.
 * Booking success - Product, upgrade and booking info required.
 
### How?

At an application level we should not be concerned with how data is tracked beyond calling our tracking module with the correct parameters, once this has been done we can trust that the tracking module will do its job. Currently we have more than one tracking system and have plans to change how these are implemented if at all. Abstracting the decision away from an application level allows changes to our current systems and implementing new systems to be much easier.

### Who?

Everyone in the business should have easy access to application tracking results and reports, to allow for quick educated choices when performing tasks such as prioritising work, assessing severity of bugs or deciding whether to consider legacy users.

## Current situation

* Not all events have tracking.
* Different events are tracked using different systems in a single application.
* Different applications have varying levels of tracking.
* Different areas of the business prefer to use different systems for reporting.

## Current goal

* One tracking module to provide an agnostic API for all our application tracking needs.
* All our applications / modules that require tracking will use the same tracking module.
* Our current tracking systems will sit behind this thin layer.
* Decide on tracking event data schema, so we are not bound to one or more current systems.
* Each tracking event will be sent to all current systems (actual data may vary, base on system).
* Different reporting systems can still be used.

## Next steps

* Review current tracking systems in use.
* Potentially change how chosen tracking systems are fired from the tracking module.
* Review how tracking data enters the data pipeline.

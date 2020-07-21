## The Mediator Pattern:
* Mediator is a behavioral design pattern that allows us to expose a unified interface through which the different parts of a system can communicate.

* If it appears that a system has too many direct relationships between the components, then it might be better to have a central point of control that components communicate throught instead.

* A real-world analogy could be a typical airport traffic control system. A tower (Mediator) handles what planes can take off and land because all communications (notifications being listened out for or broadcast) are done from the planes to the control tower, rather than from plane-to-plane. A centralized controller is key to the success of this system and that's really the role a Mediator plays in software design.

* The largest benefit of Mediator is that it reduces the communication channels needed between objects or components in a system from *many to many* to just *many to one*. Adding new publisher or subscriber is relatively easy due to the level of decoupling.

* The biggest downside of using the pattern is that it can introduce single point of failure.

* Placing a Mediator between components can also cause a performance hit as they're always communicating indirectly.

## Similarities & Difference between Event Aggregators & Mediator Patterns:
* The event aggregator, as a pattern, is designed to deal with events. The mediator, albeit, only uses them because it's convenient.

* In Event aggregators, all workflow and business logic that needs to be kicked off is put directly into the object that tiggers the events and the objects that handle the events.

* In the case of Mediator, the business logic and workflow is aggregated into the Mediator itself.

* An event aggregator facilitates a "fire & forget" model of communication. The object triggering the events doesn't care if there're any subscribers.

* A Mediator is not "fire & forget". A Mediator pays attention to a known set of input or activities so that it can facilitate and coordinate additional behavior with a known set of actors(objects).

## The Flyweight Pattern:
* The pattern aims to minimize the use of memory in an application by sharing as much data as possible with related objects (e.g. application configuration, state and so on).

* In practice, Flyweight data sharing can involve taking several similar objects or data constructs used by a number of objects and placing this data into a single external object. We can pass through this object to those depending on this data, rather than storing identical data across each one.

### Flyweights and Sharing Data:
* In the Flyweight pattern there's a concept of two states - intrinsic and extrinsic.

* Instrinisic information may be required by internal methods in our objects which they absolutely cannot function without.

* Extrinsic information can however be removed and stored externally.

* Objects with the same intrinsic data can be replaced with a single shared object, created by factory method. This allows us to reduce the overall quantity of implicit data being stored quite significantly.

* The benefit of this is that we're able to keep an eye on objects that have already been instantiated so that new copies are only ever created should the intrinsic state differ from the object we already have.

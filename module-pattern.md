## In Javascript, there are several options for implementing modules. These include:

* The Module pattern
* Object Literal notation
* AMD modules
* CommonJS modules
* ECMAScript Harmony modules

## Oject Literals:
* In object literal notation, an object is described as a set of comma-separated name/value pairs enclosed in curly braces.

* Implementation:
    ```ts
    var myObjectLiteral = {
        variableKey: variableValue,
        functionKey: function () {
            // ...
        }
    };
    ```

## The Module Pattern:
* The module pattern was originally created as a way to provide both public and private encapsulation for classes in conventional Software engineering.

* In Javascript, the `Module Pattern` is used to further *emulate* the concept of classes in such a way that we can include both public and private methods and variables inside a single object, thus shielding a particular logic from global scope.

* The pattern utilizes an *immediately-invoked function expression*(`IIFE`).

* It should be noted that there isn't really a true sense of privacy inside Javascript as it doesn't have access modifiers. Variables or methods can't technically be created using `public` or `private` access modifier.

* Implementation:
    ```ts
    var testModule = (function() {
        var counter = 0;
        return {
            incrementCounter: function() {
                return counter++;
            },

            resetCounter: function() {
                console.log(`counter value prior to reset: ${counter}`);
                counter = 0;
            }
        };
    })();

    // Increment our counter
    testModule.incrementCounter();

    // Check the counter value and reset
    // Outputs: counter value prior to reset: 1
    testModule.resetCounter();
    ```
    Here, other parts of the code are unable to directly read the value of `incrementCounter` or `resetCounter`. The counter variable is actually shielded from our global scope, acting just like a private variable.

    The module created is self-sustained in global variable `testModule`.

## The Revealing Pattern:
<span style="color:red">Description to be added.</span>
## The Singleton Pattern:
* The singleton pattern can be implemented by creating a class with a method that creates a new instance of the class if one doesn't exist. In the event of one already existing, it simply returns a reference to that object.

* Implementation in JS:
    ```ts
    const mySingleton = (function() {
        let instance;

        function init() {
            function privateMethod() {
                console.log(`I'm a private method`);
            }

            let privateVariable = `I'm a private variable`;
            let privateRandomNumber = Math.random();

            return {
                publicMethod: function() {
                    console.log(`I'm a public method`);
                },
                publicProperty: `I'm a public property`,
                getRandomNumber: function() {
                    return privateRandomNumber;
                }
            };
        }

        return {
            getInstance: function() {
                if(!instance) {
                    instance = init();
                }
                return instance;
            }
        }
    }());

    let singletonInstanceA = mySingleton.getInstance();
    let singletonInstanceB = mySingleton.getInstance();
    console.log(singletonInstanceA.getRandomNumber() === singletonInstanceB.getRandomNumber()); // true
    ```

* What makes the above code-snippet a singleton is the global access to the instance(through `getInstance()`).

* In the *GoF(Gang of Four)* book, the applicability of the singleton pattern is described as follows:

    * There must be exactly one instance of a class, and it must be accessible to clients from a well-known access point.

    * When the sole instance should be extensible by subclassing, and clients should be able to use an extended instance without modifying their code.

    * The second of these points refers to a case where we might need code such as:
        ```ts
        mySingleton.getInstance = function() {
            if(this._instance === null) {
                if(isFoo()) {
                    this._instance = new FooSingleton();
                } else {
                    this._instance = new BasicSingleton();
                }
            }
            return this._instance;
        };
        ```
        Here, `getInstance` becomes a little like a Factory methodand we don't need to update each point in our code accessing it.

## The Observer Pattern:
* The Observer pattern offers a subscription model in which objects subscribe to an event and get notified when the event occurs.

* This pattern is the cornerstone of event driven programming.

* The Observer pattern facilitates good object-oriented design and promotes loose coupling.

* The definition of the Observer pattern provided in the *GoF* book, *Design Patterns: Elements of Reusable Object-Oriented Software*, is:

    > One or more observers are interested in the state of a subject and register their interest with the subject by attaching themselves. When something changes in our subject that the observer may be interested in, a notify message is sent which calls the update method in each observer. When the observer is no longer interested in the subject's state, they can simply detach themselves.


* Participants:
    * Subject:
        * Maintains a list of observers. Any number of Observer objects may observe a Subject.
        * Implements an interface that lets Observer objects subscribe/unsubscribe to the Subject.
        * Sends a notification to the Observer objects when its state changes.

    * Observer:
        * Has a function signature that can be invoked when Subject's state changes(i.e. event occurs).

* Implementation:
    ```ts
    // This is the object that once its state changes, it will notify all the observers.
    function Observable() {
        this.observersList = [];
    }

    Observable.prototype.addObserver = function(observer) {
        this.observersList.push(observer);
    }

    Observable.prototype.removeObserver = function(observer) {
        var self = this;
        this.observersList.forEach(function(el, index) {
            if (el === observer) {
                self.observersList.splice(index, 1);
            }
        })
    }

    Observable.prototype.notify = function(context) {
        for (var i = 0; i < this.observersList.length; i++) {
            this.observersList[i].update(context);
        }
    }

    // will get notified if any changes happend on Observable
    function Observer(name) {
        this.name = name;
        var self = this;
        this.update = function(context) {
            console.log(`${self.name} is invoked with the context ${context}`);
        }
    }

    var observable = new Observable();

    var observerA = new Observer("A");
    var observerB = new Observer("B");

    observable.addObserver(observerA);
    observable.addObserver(observerB);

    observable.notify("hello");
    // should log
    // A is invoked with the context as hello
    // B is invoked with the context as hello

    observable.removeObserver(observerA);

    observable.notify("hello");
    // should log
    // B is invoked with the context as hello
    ```

## Difference between The Observer and Publish/Subscribe Pattern:
* The Observer pattern requires that the observer (or object) wishing to receive topic notifications must subscribe this interest to the object firing the event (the subject).

* The Publish/Subscribe pattern however uses a topic/event channel which sits between the objects wishing to receive notifications (subscribers) and the object firing the event (the publisher).

* The observer pattern is mostly implemented synchronously, i.e. the Subject calls the appropriate method of all its observers when an event occurs. The publisher-subscriber pattern is mostly implemented asynchronously (using a message queue).

* The observer pattern needs to be implemented in a single-application address space. On the other hand, the publisher-subscriber pattern is more of a cross-application pattern.

* The con of using Observer pattern is Observables only maintain one array for keeping observers(in the above example, the array is `observersList`). It doesn't differentiate how the update is triggered because it only has one `notify` function, which triggers all the functions stored in the array.

* If we want to group observers handlers based on different events. We just need to modify that `observersList` to an object like,
    ```ts
    let events = {
        'event1': [handler1, handler2],
        'event2': [handler2, handler3]
    };
    ```
and, this variation is generally known as `pub/sub`. So you can trigger different functions based on the `events` you published.

* [Observer vs Pub/Sub Pattern](https://medium.com/better-programming/observer-vs-pub-sub-pattern-50d3b27f838c#:~:text=In%20the%20observer%20pattern%2C%20the,message%20queues%20or%20a%20broker.)

* [Difference between Observer & Pub/Sub pattern](https://stackoverflow.com/a/38275499/6352772)


## Publish/Subscribe Pattern:
* Publish/Subscribe fits in very well in JavaScript ecosystems, largely because at the core, ECMAScript implementations are event driven.

* In the Publisher/Subscriber pattern, senders of messages, called publishers, do not program the messages to be sent directly to specific receivers, called subscribers.

* This means that Publisher and Subscribers don't know about each others identities.

* There's generally a third component involved, called *broker*, *message broker*, or *event bus*, which is known by both Publisher and Subscribers.

* A broker filters out the messages on the basis of several processes, topic-based, content-based being the most popular ones.

* Implementation for Pub/Sub:
    ```ts
    function Pubsub() {
        this.events = {};
    }

    Pubsub.prototype.publish = function(event, args) {
        if(!this.events[event]) {
            return false;
        }

        const subscribers = this.events[event];
        const len = subscribers ? subscribers.length : 0;

        while (len--) {
            subscribers[len]();
        }
        return this;
    }

    Pubsub.prototype.subscribe = function(event, func) {
        if(!this.events[event]) {
            this.events[event] = [];
        }
        this.events[event].push(func);
    }

    Pubsub.prototype.unsubscribe = function(event, func) {
        const self = this;
        if(this.events[event]) {
            this.events[event].forEach(function(el, index) {
                if (el == func) {
                    self.events.splice(index, 1);
                }
            })
        }
    }

    const pubsub = new Pubsub();

    const eat = function() {
        console.log("I am eating");
    }

    const drink = function() {
        console.log("I am drinking");
    }

    const running = function() {
        console.log("I am running");
    }

    pubsub.subscribe("dinner", eat);
    pubsub.subscribe("dinner", drink);
    pubsub.subscribe("sports", running);

    pubsub.publish("dinner");
    // should log
    // I am drinking
    // I am eating

    pubsub.publish("sports")
    // should log
    // I am running
    ```

* References:
    * https://dzone.com/articles/design-patterns-mediator

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

## The Prototype Pattern:
* The *GoF* refer to the prototype pattern as one which creates objects based on a template of an existing object through cloning.

* We can think of the prototype pattern as being based on prototypal inheritance where we create objects which act as the prototype for other objects. The prototype object itself is effectively used a blueprint for each object the constructor creates.

* One of the benefits of using the prototypal pattern is that we're working with the prototypal strengths Javascript has to offer natively rather than attempting to imitate features of other languages.

* Real prototypal inheritance, as described in ECMAScript 5 standard, requires the use of `Object.create`.

* Implementation:
    ```ts
    const Car = {
        name: "Ford Escort",

        drive: function() {
            console.log(`See, I'm driving!`);
        }

        panic: function() {
            console.log('Holy sh*t!');
        }
    }

    const myCar = Object.create(Car);
    console.log(myCar.name);
    ```

* `Object.create` also allows us to readily implement advanced concepts such as differential inheritance where objects are able to directly inherit from another objects.

* `Object.create` also allows us to initialise object properties using the second supplied argument. For example:
    ```ts
    const myCar = Object.create(Car, {
        'id': {
            value: 'CAR_001',
            // writable: false,
            // configurable: false
            enumerable: true
        }
    });
    ```

## The Command Pattern:
* Command decouples the object that invokes the operation from the one that knows how to perform it.

* To achieve this separation, the designer creates an absract base class that maps a receiver(an object) with an action(a pointer to a member function). The base class contains an `execute` method that simply calls the action on the receiver.

* All clients of Command objects treat each object as "black box" by simply invoking the object's virtual `execute()` method whenever the client requires the object's service.

* A command class holds some subset of the following: an object, a method to be applied to the object, and the arguments to be passed when the method is applied. The command's `execute` method then causes the pieces to come together.

* The client that creates a command is not the same client that executes it. This separation provides flexibilty in the timing and sequencing of commands.

* Materlializing commands as objects means they can be passed, staged, shared, loaded in a table, and otherwise instrumented or manipulated like any other object.

* Command objects can be thought of as "tokens" that are created by one client that knows what need to be done, and passed to another client that has the resources for doing it.

* Implementation:
    ```ts
    const carManager = {
       // request information
        requestInfo: function(model, id) {
            return `The information for ${model} with ID ${id} is foobar`;
        },

        // purchase the car
        buyVehicle: function(model, id) {
            return `You have successfully purchased Item ${id}, a${model}`;
        },

        // arrange a viewing
        arrangeViewing: function(model, id) {
            return `You have successfully booked a viewing of ${model} (${id})`;
        }
    };

    carManager.execute = function(name) {
        return carManager[name] && carManager[name].apply(carManager, [].slice.call(arguments, 1));
    };

    carManager.execute("arrangeViewing", "Ferrari", "14523");
    carManager.execute("requestInfo", "Ford Mondeo", "54323");
    ```

* References:
    * https://sourcemaking.com/design_patterns/command

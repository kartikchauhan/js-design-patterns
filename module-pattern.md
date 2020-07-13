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

* The con of using Observer pattern is Observables only maintain one array for keeping observers(in the above example, the array is `observersList`). It doesn't differentiate how the update is triggered because it only has one `notify` function, which triggers all the functions stored in the array.

* If we want to group observers handlers based on different events. We just need to modify that `observersList` to an object like,
    ```ts
    let events = {
        'event1': [handler1, handler2],
        'event2': [handler2, handler3]
    };
    ```
and, this variation is generally known as `pub/sub`. So you can trigger different functions based on the `events` you published.

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

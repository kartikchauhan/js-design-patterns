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

    var observerA = new Observer('A');
    var observerB = new Observer('B');

    observable.addObserver(observerA);
    observable.addObserver(observerB);

    observable.notify('hello');
    // should log
    // A is invoked with the context as hello
    // B is invoked with the context as hello

    observable.removeObserver(observerA);

    observable.notify('hello');
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

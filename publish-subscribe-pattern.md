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
        console.log('I am eating');
    }

    const drink = function() {
        console.log('I am drinking');
    }

    const running = function() {
        console.log('I am running');
    }

    pubsub.subscribe('dinner', eat);
    pubsub.subscribe('dinner', drink);
    pubsub.subscribe('sports', running);

    pubsub.publish('dinner');
    // should log
    // I am drinking
    // I am eating

    pubsub.publish('sports')
    // should log
    // I am running
    ```

* References:
    * https://dzone.com/articles/design-patterns-mediator

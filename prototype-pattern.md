## The Prototype Pattern:
* The *GoF* refer to the prototype pattern as one which creates objects based on a template of an existing object through cloning.

* We can think of the prototype pattern as being based on prototypal inheritance where we create objects which act as the prototype for other objects. The prototype object itself is effectively used a blueprint for each object the constructor creates.

* One of the benefits of using the prototypal pattern is that we're working with the prototypal strengths Javascript has to offer natively rather than attempting to imitate features of other languages.

* Real prototypal inheritance, as described in ECMAScript 5 standard, requires the use of `Object.create`.

* Implementation:
    ```ts
    const Car = {
        name: 'Ford Escort',

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

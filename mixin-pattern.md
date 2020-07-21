## The Mixin Pattern:
* In traditional programming languages such as C++ and Lisp, Mixins are classes which offer functionality that can be easily inherited by a sub-class or group of sub-classes for the purpose of function re-use.

* *Sub-classing* is a term that refers to inheriting properties for a new object from a base or superclass object.

* example:
    ```ts
    const Person = function (firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.gender = 'male';
    };

    // a new instance of Person can then easily be created as follows:
    const clark = new Person('Clark', 'Kent');

    // Define a subclass constructor for for "Superhero":
    const Superhero = function (firstName, lastName, powers) {
        // Invoke the superclass constructor on the new object
        // then use .call() to invoke the constructor as a method of
        // the object to be initialized.
        Person.call(this, firstName, lastName);

        // Finally, store their powers, a new array of traits not found in a normal "Person"
        this.powers = powers;
    };

    Superhero.prototype = Object.create(Person.prototype);
    const superman = new Superhero('Clark', 'Kent', ['flight', 'heat-vision']);
    console.log(superman);

    // Outputs Person attributes as well as powers
    ```
    The `Superhero` constructor creates an oject which descends from `Person`.

### Mixins:
* Each new object we define has a prototype from which it can inherit further properties.

* Prototypes can inherit from other object prototypes but, even more importantly, can define properties for any number of object instances. This can be leveraged to promote function re-use.

* example:
    ```ts
    const myMixins = {
        moveUp: function() {
            console.log("move up");
        },
        moveDown: function() {
            console.log("move down");
        },
        stop: function() {
            console.log("stop! in the name of love!");
        }
    };
    ```
    We can then easily extend the prototype of existing constructor functions to include this behavior using a helper such as the lodash `_.extend()` method:

    ```ts
    // A skeleton carAnimator constructor
    function CarAnimator() {
        this.moveLeft = function () {
            console.log("move left");
        };
    }

    // A skeleton personAnimator constructor
    function PersonAnimator() {
        this.moveRandomly = function () { /*..*/ };
    }

    // Extend both constructors with our Mixin
    _.extend(CarAnimator.prototype, myMixins);
    _.extend(PersonAnimator.prototype, myMixins);

    // Create a new instance of carAnimator
    var myAnimator = new CarAnimator();
    myAnimator.moveLeft();
    myAnimator.moveDown();
    myAnimator.stop();

    // Outputs:
    // move left
    // move down
    // stop! in the name of love!
    ```

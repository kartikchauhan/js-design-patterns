## In Javascript, there are several options for implementing modules. These include:

 * The Module pattern
 * Object Literal notation
 * AMD modules
 * CommonJS modules
 * ECMAScript Harmony modules

## Oject Literals:
In object literal notation, an object is described as a set of comma-separated name/value pairs enclosed in curly braces.

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

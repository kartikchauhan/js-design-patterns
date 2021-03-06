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

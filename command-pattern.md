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

    carManager.execute('arrangeViewing', 'Ferrari', '14523');
    carManager.execute('requestInfo', 'Ford Mondeo', '54323');
    ```

* Submit transaction either using CLI or any sdk in Hyperledger Fabric can be thought of as a good example of the command pattern.

    `await contract.submitTransaction('createCar', 'CAR12', 'Honda', 'Accord', 'Black', 'Tom');`

* References:
    * https://sourcemaking.com/design_patterns/command

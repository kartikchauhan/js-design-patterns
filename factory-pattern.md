## The Factory Pattern:
* A creational pattern, that doesn't explicitly require us to use a constructor. Instead a Factory can provide a generic interface for creating objects, where we can specify the type of factory object we wish to be created.

* Imagine that we have a UI factory where we are asked to create a type of UI component. Rather than creating this component directly using the `new` operator or via another creational pattern, we ask a Factory object for a new component instead. We inform the Factory what type of object is required(eg. Button, panel) and it instantiates it for us.

* This is particulary helpful if the object creation process is relatively complex.

* Implementation:
    ```ts
    // A constructor for defining new cars
    function Car(options) {
        // some defaults
        this.doors = options.doors || 4;
        this.state = options.state || 'brand new';
        this.color = options.color || 'silver';

    }

    // A constructor for defining new trucks
    function Truck(options) {
        this.state = options.state || 'used';
        this.wheelSize = options.wheelSize || 'large';
        this.color = options.color || 'blue';
    }

    // FactoryExample.js

    // Define a skeleton vehicle factory
    function VehicleFactory() { }

    // Define the prototypes and utilities for this factory

    // Our default vehicleClass is Car
    VehicleFactory.prototype.vehicleClass = Car;

    // Our Factory method for creating new Vehicle instances
    VehicleFactory.prototype.createVehicle = function(options) {

        switch (options.vehicleType) {
            case 'car':
                this.vehicleClass = Car;
                break;
            case 'truck':
                this.vehicleClass = Truck;
                break;
            //defaults to VehicleFactory.prototype.vehicleClass (Car)
        }

        return new this.vehicleClass(options);

    };

    // Create an instance of our factory that makes cars
    var carFactory = new VehicleFactory();
    var car = carFactory.createVehicle({
        vehicleType: 'car',
        color: 'yellow',
        doors: 6
    });

    // Test to confirm our car was created using the vehicleClass/prototype Car

    // Outputs: true
    console.log(car instanceof Car);

    // Outputs: Car object of color "yellow", doors: 6 in a "brand new" state
    console.log(car);
    ```

    Approach #1: Modify a `VehicleFactory` instance to use the `Truck` class.
    ```ts
    var movingTruck = carFactory.createVehicle({
        vehicleType: "truck",
        state: "like new",
        color: "red",
        wheelSize: "small"
    });

    // Test to confirm our truck was created with the vehicleClass/prototype Truck

    // Outputs: true
    console.log(movingTruck instanceof Truck);

    // Outputs: Truck object of color "red", a "like new" state
    // and a "small" wheelSize
    console.log(movingTruck);
    ```

    Approach #2: Subclass `VehicleFactory` to create a factory class that build Trucks
    ```ts
    function TruckFactory() { }

    TruckFactory.prototype = new VehicleFactory();
    TruckFactory.prototype.vehicleClass = Truck;

    var truckFactory = new TruckFactory();
    var myBigTruck = truckFactory.createVehicle({
        state: "omg..so bad.",
        color: "pink",
        wheelSize: "so big"
    });

    // Confirms that myBigTruck was created with the prototype Truck
    // Outputs: true
    console.log(myBigTruck instanceof Truck);

    // Outputs: Truck object with the color "pink", wheelSize "so big"
    // and state "omg. so bad"
    console.log(myBigTruck);
    ```

## Difference between Factory and Constructor pattern?:
They both are there to create instance of an object.

```ts
ElementarySchool school = new ElementarySchool();
ElementarySchool school = SchoolFactory.Construct(); // new ElementarySchool() inside
```

No difference so far. Now imagine that we have various school types and we want to switch from using ElementarySchool to HighSchool (which is derived from an ElementarySchool or implements the same interface ISchool as the ElementarySchool). The code change would be:

```ts
HighSchool school = new HighSchool();
HighSchool school = SchoolFactory.Construct(); // new HighSchool() inside
```

In case of an interface we would have:

```ts
ISchool school = new HighSchool();
ISchool school = SchoolFactory.Construct(); // new HighSchool() inside
```

Now if you have this code in multiple places you can see that using factory method might be pretty cheap because once you change the factory method you are done (if we use the second example with interfaces).

And this is the main difference and advantage. When you start dealing with a complex class hierarchies and you want to dynamically create an instance of a class from such a hierarchy you get the following code. Factory methods might then take a parameter that tells the method what concrete instance to instantiate. Let's say you have a MyStudent class and you need to instantiate corresponding ISchool object so that your student is a member of that school.

```ts
ISchool school = SchoolFactory.ConstructForStudent(myStudent);
```

Now you have one place in your app that contains business logic that determines what ISchool object to instantiate for different IStudent objects.

So - for simple classes (value objects, etc.) constructor is just fine (you don't want to overengineer your application) but for complex class hierarchies factory method is a preferred way.

### References:
* https://stackoverflow.com/a/629006/6352772

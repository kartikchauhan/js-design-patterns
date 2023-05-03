## The three common ways to create new objects in JS are as follows:

```ts
var newObject = {};
 
// or
var newObject = Object.create( Object.prototype );
 
// or
var newObject = new Object();
```

## Basic Constructors:
```ts
function Car(model, year, miles) {
    // this refers to the new object that is being created.
    this.model = model;
    this.year = year;
    this.miles = miles;
 
    this.toString = function () {
        return this.model + " has done " + this.miles + " miles";
    };
}
 
var civic = new Car("Honda Civic", 2009, 20000);
var mondeo = new Car("Ford Mondeo", 2010, 5000);
 
console.log(civic.toString());
console.log(mondeo.toString());
```

* The problem with above method is that every object will have its own copy of `toString` method which is not very efficient.

## Constructors With Prototypes:
```ts
function Car(model, year, miles) {
    this.model = model;
    this.year = year;
    this.miles = miles; 
}

Car.prototype.toString = function () {
    return this.model + " has done " + this.miles + " miles";
};
 
var civic = new Car("Honda Civic", 2009, 20000);
var mondeo = new Car("Ford Mondeo", 2010, 5000);
 
console.log(civic.toString());
console.log(mondeo.toString());
```

* Adding method `toString` to the prototype of function `Car` will make the method available to all its instances.

* Every instance will share the same copy of `toString` method.

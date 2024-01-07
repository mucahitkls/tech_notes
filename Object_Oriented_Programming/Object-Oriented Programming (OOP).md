## Introduction to Object-Oriented Programming

Object-Oriented Programming (OOP) is a programming paradigm centered around objects rather than actions. It's a method of structuring a program by bundling related properties and behaviors into individual objects. Essentially, OOP models real-world entities as software objects that have some data associated with them and can perform certain functions.


![[Object_oriented_programming.png#center|500]]



## Understanding OOP

### Definition:

- **Object-Centric**: In OOP, the focus is on objects that represent abstract or concrete things of the real world. These objects are first defined by their character (data) and their behavior (functions).

### Purpose:

- **Modularity**: The source code for an object can be written and maintained independently of the source code for other objects.
- **Reusability**: Objects can be reused across programs.
- **Scalability**: OOP allows for scalability and code manageability as the project grows in size and complexity.
## Key Concepts of OOP

### 1. [[Class in Object-Oriented Programming (OOP)|Class]]:

- A blueprint or template for creating objects. It provides the initial values for state (member variables or attributes) and implementations of behavior (member functions or methods).

### 2. [[Object-Oriented Programming (OOP)|Object]]:

- An instance of a class. It is a basic unit of OOP and represents real-life entities.

### 3. [[Method in Object-Oriented Programming (OOP)|Methods]]:

- Functions that are defined inside a class and describe the behaviors of an object.

### 4. [[Attribute-Property in Object-Oriented Programming (OOP)|Attributes/Properties]]:

- Variables that hold data specific to an object.

### 5. [[Inheritance in Object-Oriented Programming (OOP)|Inheritance]]:

- A mechanism where a new class (derived class) inherits the attributes and methods of another class (base class).

### 6. [[Polymorphism in Object-Oriented Programming (OOP)|Polymorphism]]:

- The ability to present the same interface for different data types. Methods can perform different tasks based on the object that it is operating on.

### 7. [[Encapsulation in Object-Oriented Programming (OOP)|Encapsulation]]:

- The idea of bundling data and methods that work on that data within one unit, e.g., a class in OOP. It hides the internal state of the object from the outside.

### 8. [[Abstraction in Object-Oriented Programming (OOP)|Abstraction]]:

- Hiding the complex reality while exposing only the necessary parts. It reduces complexity and isolates the impact of changes.

## Advanced Aspects of OOP

### 1. [[Association in Object-Oriented Programming (OOP)|Association]], [[Aggregation in Object-Oriented Programming (OOP)|Aggregation]], [[Composition in Object-Oriented Programming (OOP)|Composition]]:

- Various ways objects can be associated with each other, with varying degrees of ownership and lifecycle control.

### 2. Design Patterns:

- Standard solutions to common problems in software design. E.g., Factory, Singleton, Observer patterns.

### 3. [[Design Principles in Software Development|SOLID Principles]]:

- A set of design principles for OOP that includes Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, and Dependency Inversion.

## JavaScript OOP Code Examples

### Defining a Class:

```javascript
class Car {
    constructor(brand, model) {
        this.brand = brand;
        this.model = model;
    }

    displayInfo() {
        console.log(`This is a ${this.brand} ${this.model}.`);
    }
}

// Creating an object
let myCar = new Car("Tesla", "Model X");
myCar.displayInfo();  // This is a Tesla Model X.
```

### Inheritance:

```javascript
class ElectricCar extends Car {
    constructor(brand, model, batteryCapacity) {
        super(brand, model);
        this.batteryCapacity = batteryCapacity;
    }

    displayBatteryInfo() {
        console.log(`It has a battery capacity of ${this.batteryCapacity}.`);
    }
}

// Using the inherited class
let myElectricCar = new ElectricCar("Tesla", "Model 3", "75 kWh");
myElectricCar.displayInfo();        // This is a Tesla Model 3.
myElectricCar.displayBatteryInfo(); // It has a battery capacity of 75 kWh.
```

## Conclusion

Object-Oriented Programming is a powerful paradigm that helps manage and reduce complexity in software design. It provides mechanisms to structure a program in a clean, modular, and scalable way. Understanding and leveraging the power of OOP principles like inheritance, polymorphism, encapsulation, and abstraction can lead to more effective and efficient coding practices. As you delve deeper into OOP, the nuances and advanced concepts will unlock even greater potential for writing high-quality code.

## Introduction to Abstraction in OOP

Abstraction in Object-Oriented Programming is a fundamental concept that focuses on hiding the complex reality while exposing only the necessary parts. It's about making complex systems simple by representing them with abstract entities.

See [[Cloud Computing Abstractions]] article for another abstraction approach.

## Understanding Abstraction in OOP

### Definition:

- **Simplifying Complexity**: Abstraction is the concept of taking a complex system and making it easier to manage by providing a simplified model.

### Purpose:

- **Manageability and Usability**: Allows developers to work with concepts at a higher level, making it easier to manage complexity and build upon the foundations.

## Key Concepts of Abstraction in OOP

### 1. Abstract Classes:

- Classes that cannot be instantiated and are often used to create a base class with default functionality and structure.

### 2. Interfaces:

- Defines a contract or a skeleton structure that other classes must follow, without providing the method implementations.

### 3. Hiding Implementation Details:

- Showing only the essential features and hiding the detailed implementation from the user.

## Advanced Aspects of Abstraction in OOP

### 1. Abstract Methods:

- Methods declared in an abstract class or interface but must be defined in the subclass.

### 2. Encapsulation vs Abstraction:

- While both aim to simplify complex systems, encapsulation focuses on hiding the internal state, and abstraction focuses on simplifying the user interaction.

### 3. Inheritance and Abstraction:

- Abstract classes can be used as a base for other classes, defining a template for other classes to follow.

### 4. Polymorphism and Abstraction:

- Polymorphism can be achieved through abstraction by allowing objects to be treated as instances of their abstract base class.

## JavaScript Abstraction Example

In JavaScript, abstraction can be achieved using classes and interfaces (in TypeScript or with the use of libraries).

### Before Abstraction:

```javascript
class Car {
    startEngine() {
        console.log("Engine started.");
    }

    stopEngine() {
        console.log("Engine stopped.");
    }

    turnOnHeadlights() {
        console.log("Headlights turned on.");
    }

    playMusic() {
        console.log("Music started playing.");
    }
}

// Using the class
let myCar = new Car();
myCar.startEngine();
myCar.turnOnHeadlights();
myCar.playMusic();
```

### After Abstraction:

```javascript
// Abstracting vehicle operations
class Vehicle {
    startEngine() {
        console.log("Engine started.");
    }

    stopEngine() {
        console.log("Engine stopped.");
    }
}

// Car now only needs to focus on what's unique to it
class Car extends Vehicle {
    turnOnHeadlights() {
        console.log("Headlights turned on.");
    }

    playMusic() {
        console.log("Music started playing.");
    }
}

// Using the abstracted class
let myCar = new Car();
myCar.startEngine();
myCar.turnOnHeadlights();
```

## Python Abstraction Example

Python supports abstraction through abstract base classes in the `abc` module.

### Before Abstraction:

```python
class Car:
    def start_engine(self):
        print("Engine started.")
    
    def stop_engine(self):
        print("Engine stopped.")
    
    def turn_on_headlights(self):
        print("Headlights turned on.")

    def play_music(self):
        print("Music started playing.")

# Using the class
my_car = Car()
my_car.start_engine()
my_car.turn_on_headlights()
my_car.play_music()
```

### After Abstraction:

```python
from abc import ABC, abstractmethod

class Vehicle(ABC):
    @abstractmethod
    def start_engine(self):
        pass
    
    @abstractmethod
    def stop_engine(self):
        pass

class Car(Vehicle):
    def start_engine(self):
        print("Engine started.")
    
    def stop_engine(self):
        print("Engine stopped.")
    
    def turn_on_headlights(self):
        print("Headlights turned on.")

    def play_music(self):
        print("Music started playing.")

# Using the abstracted class
my_car = Car()
my_car.start_engine()
my_car.turn_on_headlights()
```


See an extensive [[Coding Example of Abstraction|example]].
## Conclusion

Abstraction is a powerful principle in object-oriented programming that allows developers to reduce complexity by exposing only the necessary parts of the system and hiding the implementation details. It makes the application easier to manage, extend, and use. Properly understanding and implementing abstraction leads to cleaner, more efficient, and maintainable code. As you continue to explore and apply abstraction, you'll find it an invaluable tool in designing and implementing complex systems, making your development process more streamlined and effective.
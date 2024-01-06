
## Introduction to Inheritance in OOP

Inheritance is a fundamental concept in Object-Oriented Programming (OOP) that allows a new class to receive, or inherit, properties and methods from existing classes. This promotes code reusability and establishes a hierarchical relationship between classes.

## Understanding Inheritance in OOP

### Definition:

- **Hierarchical Class System**: Inheritance allows the creation of a new class (derived or child class) that inherits the attributes and methods of an existing class (base or parent class).

### Purpose:

- **Code Reusability**: Promotes reusability by allowing new classes to be built upon existing classes without the need to duplicate code.
- **Hierarchical Structure**: Establishes a clear hierarchy, which can simplify complex relationships and promote understanding.

## Key Concepts of Inheritance in OOP

### 1. Base/Parent Class:

- The class whose properties and methods are inherited by another class.

### 2. Derived/Child Class:

- The class that inherits properties and methods from another class.

### 3. Overriding:

- A child class can provide a specific implementation of a method that is already defined in its parent class.

### 4. Super/Parent Reference:

- A reference to the parent class, often used to access parent class methods and constructors.

## Advanced Aspects of Inheritance in OOP

### 1. Multiple Inheritance:

- A feature where a class can inherit features from more than one parent class. Note: This is not supported in all OOP languages (JavaScript does not support it).

### 2. Multilevel Inheritance:

- A feature where a class can inherit from a derived class, making it the base class for a new class.

### 3. Hierarchical Inheritance:

- A feature where multiple classes inherit from a single class.

### 4. Abstract Classes:

- Classes that cannot be instantiated on their own and are often used as base classes.

### 5. Interfaces:

- Defines a contract that implementing classes must follow, without providing the method implementation.

## JavaScript and Inheritance

JavaScript uses prototypal inheritance. However, ES6 introduced the `class` keyword, making it easier to use inheritance in a way that's familiar to developers from classical OOP languages.

### Basic Inheritance:

```javascript
class Animal {
    constructor(name) {
        this.name = name;
    }

    speak() {
        console.log(`${this.name} makes a noise.`);
    }
}

class Dog extends Animal {
    speak() {
        console.log(`${this.name} barks.`);
    }
}

let dog = new Dog('Rex');
dog.speak();  // Output: Rex barks.
```

### Before Inheritance:

Without inheritance, you would have to duplicate the methods and properties for each related class.

```javascript
class Animal {
    constructor(name) {
        this.name = name;
    }
    
    speak() {
        console.log(`${this.name} makes a noise.`);
    }
}

class Dog {
    constructor(name) {
        this.name = name;
    }
    
    speak() {
        console.log(`${this.name} barks.`);
    }
}

let animal = new Animal('Generic Animal');
animal.speak();  // Output: Generic Animal makes a noise.

let dog = new Dog('Rex');
dog.speak();  // Output: Rex barks.
```

### After Inheritance:

With inheritance, `Dog` class can inherit from `Animal`, reducing code duplication.

```javascript
class Animal {
    constructor(name) {
        this.name = name;
    }
    
    speak() {
        console.log(`${this.name} makes a noise.`);
    }
}

class Dog extends Animal {
    speak() {
        super.speak(); // Calls the speak method from Animal
        console.log(`${this.name} barks.`);
    }
}

let dog = new Dog('Rex');
dog.speak();  // Output: Rex makes a noise. Rex barks.
```


See an extensive [[Real-Life Coding Example of Inheritance|example]].

## Conclusion

Inheritance is a powerful feature of OOP that promotes code reusability and establishes a natural hierarchy in applications. It allows developers to create new classes that are built upon existing classes, reducing redundancy and fostering a clean, understandable structure. Advanced features of inheritance, like abstract classes and interfaces, further enhance the capability to design robust and flexible systems. While JavaScript has its unique prototypal inheritance model, the introduction of classes and extends keywords has made it more approachable for those familiar with classical OOP concepts. As you continue to explore and apply inheritance, you'll find it an indispensable tool in your OOP toolkit, enabling you to create more sophisticated and maintainable applications.
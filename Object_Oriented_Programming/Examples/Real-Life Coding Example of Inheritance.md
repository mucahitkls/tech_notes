# JavaScript Inheritance Example

## Scenario: Online Retail Store System

**Objective**: We are creating a system for an online retail store. The store sells various types of products, each with different properties and discount methods.

### Without Inheritance:

Initially, we might design separate classes for each product type, replicating similar functionalities across them.

```javascript
class Electronic {
    constructor(name, price) {
        this.name = name;
        this.price = price;
    }

    applyDiscount() {
        this.price = this.price - (this.price * 0.1); // 10% discount for all electronics
    }
}

class Clothing {
    constructor(name, price, size) {
        this.name = name;
        this.price = price;
        this.size = size;
    }

    applyDiscount() {
        this.price = this.price - (this.price * 0.2); // 20% discount for all clothing
    }
}

// Usage
let laptop = new Electronic("Laptop", 1000);
let tshirt = new Clothing("T-Shirt", 50, "M");

laptop.applyDiscount();
tshirt.applyDiscount();

console.log(laptop.price);  // $900 after discount
console.log(tshirt.price);  // $40 after discount
```

**Issues**: Code duplication for similar methods and attributes. If a new product type is introduced or the discount logic changes, each class needs to be updated separately.

### After Introducing Inheritance:

We introduce a base class `Product` that encapsulates common properties and methods. Each specific product class then extends this base class.

```javascript
class Product {
    constructor(name, price) {
        this.name = name;
        this.price = price;
    }

    applyDiscount() {
        // Default discount method, can be overridden
        this.price = this.price - (this.price * 0.05);
    }
}

class Electronic extends Product {
    constructor(name, price) {
        super(name, price);
    }

    applyDiscount() {
        this.price = this.price - (this.price * 0.1);
    }
}

class Clothing extends Product {
    constructor(name, price, size) {
        super(name, price);
        this.size = size;
    }

    applyDiscount() {
        this.price = this.price - (this.price * 0.2);
    }
}

// Usage
let laptop = new Electronic("Laptop", 1000);
let tshirt = new Clothing("T-Shirt", 50, "M");

laptop.applyDiscount();
tshirt.applyDiscount();

console.log(laptop.price);  // $900 after discount
console.log(tshirt.price);  // $40 after discount
```

**Improvements**: 
- Code Duplication Reduced: The common properties and methods are now centralized in the `Product` class.
- Easy to Maintain and Extend: Introducing a new product or changing the discount logic is now much simpler and requires changes in fewer places.
- Clear Hierarchy: The system now has a clear hierarchical structure, making it easier to understand and manage.

# Python Inheritance Example

## Scenario: School Management System

**Objective**: We're developing a system for managing school personnel. Both teachers and students share some common attributes but also have unique ones.

### Without Inheritance:

Initially, separate classes might be created for teachers and students, replicating shared attributes and methods.

```python
class Teacher:
    def __init__(self, name, subject):
        self.name = name
        self.subject = subject

    def getInfo(self):
        return f"Teacher: {self.name}, Subject: {self.subject}"

class Student:
    def __init__(self, name, grade):
        self.name = name
        self.grade = grade

    def getInfo(self):
        return f"Student: {self.name}, Grade: {self.grade}"

# Usage
mrSmith = Teacher("Mr. Smith", "Math")
johnDoe = Student("John Doe", "A")

print(mrSmith.getInfo())  # Teacher: Mr. Smith, Subject: Math
print(johnDoe.getInfo())  # Student: John Doe, Grade: A
```

**Issues**: Code duplication for the `getInfo` method. If changes are needed in shared attributes or methods, each class needs to be updated.

### After Introducing Inheritance:

We introduce a `Person` class that encapsulates common properties and methods. `Teacher` and `Student` classes then extend this base class.

```python
class Person:
    def __init__(self, name):
        self.name = name

    def getInfo(self):
        return f"Name: {self.name}"

class Teacher(Person):
    def __init__(self, name, subject):
        super().__init__(name)
        self.subject = subject

    def getInfo(self):
        return f"{super().getInfo()}, Subject: {self.subject}"

class Student(Person):
    def __init__(self, name, grade):
        super().__init__(name)
        self.grade = grade

    def getInfo(self):
        return f"{super().getInfo()}, Grade: {self.grade}"

# Usage
mrSmith = Teacher("Mr. Smith", "Math")
johnDoe = Student("John Doe", "A")

print(mrSmith.getInfo())  # Name: Mr. Smith, Subject: Math
print(johnDoe.getInfo())  # Name: John Doe, Grade: A
```

**Improvements**: 
- Reduced Redundancy: The `Person` class centralizes common attributes and methods.
- Enhanced Maintainability: Updating shared features or adding new types of people is more straightforward and requires fewer code changes.
- Hierarchical Structure: The system now clearly reflects the real-world relationship between people, teachers, and students.

In both examples, inheritance has transformed the design, making it more scalable, maintainable, and reflective of the real-world relationships and hierarchies. The use of inheritance has also introduced encapsulation as common properties and behaviors are encapsulated in the base `Product` and `Person` classes respectively.
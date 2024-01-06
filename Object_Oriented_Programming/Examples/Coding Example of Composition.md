# JavaScript Example of Composition

## Scenario: Building a Computer System

Let's imagine we're creating a system to represent a computer. A computer is composed of several parts like a CPU, memory, and storage.

### Without Composition:

Initially, we might define a monolithic `Computer` class that includes all functionalities, making it hard to manage and extend.

```javascript
class Computer {
    constructor(cpu, memorySize, storageSize) {
        this.cpu = cpu;
        this.memorySize = memorySize;
        this.storageSize = storageSize;
    }

    describe() {
        console.log(`This computer has a ${this.cpu}, ${this.memorySize} of RAM, and ${this.storageSize} of storage.`);
    }
}

// Usage
let myComputer = new Computer("Intel i7", "16GB", "1TB");
myComputer.describe();
```

**Issues**: This design is inflexible and tightly coupled. Adding a new component or changing an existing one affects the entire `Computer` class.

### After Introducing Composition:

We create separate classes for each component and compose a `Computer` class from these parts.

```javascript
class CPU {
    constructor(type) {
        this.type = type;
    }
}

class Memory {
    constructor(size) {
        this.size = size;
    }
}

class Storage {
    constructor(size) {
        this.size = size;
    }
}

class Computer {
    constructor(cpu, memory, storage) {
        this.cpu = new CPU(cpu);
        this.memory = new Memory(memory);
        this.storage = new Storage(storage);
    }

    describe() {
        console.log(`This computer has a ${this.cpu.type}, ${this.memory.size} of RAM, and ${this.storage.size} of storage.`);
    }
}

// Usage
let myComputer = new Computer("Intel i7", "16GB", "1TB");
myComputer.describe();
```

**Improvements**: 
- **Modularity**: Each component is a module that can be managed independently.
- **Flexibility**: It's easier to add or replace parts without affecting the whole system.
- **Maintainability**: Changes in one part do not ripple through the entire system.

# Python Example of Composition

## Scenario: Company Management System

Imagine we're building a system for managing a company with departments and employees.

### Without Composition:

Initially, we might define all functionality within a single `Company` class.

```python
class Company:
    def __init__(self, name):
        self.name = name
        self.employees = []
        self.departments = []

    def add_employee(self, employee_name):
        self.employees.append(employee_name)

    def add_department(self, department_name):
        self.departments.append(department_name)

# Usage
myCompany = Company("TechCorp")
myCompany.add_employee("John Doe")
myCompany.add_department("Development")

print(myCompany.employees)  # ['John Doe']
print(myCompany.departments)  # ['Development']
```

**Issues**: The `Company` class is doing too much, handling details of both employees and departments. It's not scalable or easy to manage.

### After Introducing Composition:

We create separate classes for `Department` and `Employee` and compose the `Company` class using these.

```python
class Employee:
    def __init__(self, name):
        self.name = name

class Department:
    def __init__(self, name):
        self.name = name
        self.employees = []

    def add_employee(self, employee):
        self.employees.append(employee)

class Company:
    def __init__(self, name):
        self.name = name
        self.departments = []

    def add_department(self, department):
        self.departments.append(department)

# Usage
myCompany = Company("TechCorp")
development = Department("Development")
employee = Employee("John Doe")

development.add_employee(employee)
myCompany.add_department(development)

print([emp.name for emp in development.employees])  # ['John Doe']
print([dept.name for dept in myCompany.departments])  # ['Development']
```

**Improvements**: 
- **Separation of Concerns**: `Employee` and `Department` are now separate from `Company`, making the system more organized.
- **Reusability**: `Employee` and `Department` can be used and managed independently, and easily integrated into other systems or parts of the application.
- **Ease of Modification**: Changes to the structure or functionality of employees or departments can be made without affecting the `Company` class.

In both JavaScript and Python examples, introducing composition simplified the system by breaking it down into smaller, manageable parts. It allowed for better organization, easier maintenance, and greater flexibility in the system design. Composition also inherently applies some level of encapsulation as the internal workings of each component are hidden from others, exposing only what's necessary. This results in a system that's not only easier to manage but also more robust and adaptable to change.
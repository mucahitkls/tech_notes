
## Introduction to Law of Demeter

The Law of Demeter (LoD), also known as the Principle of Least Knowledge, is a design guideline for developing software, particularly object-oriented programs. Formulated at Northeastern University in the late 1980s, the principle states that a unit of software should have limited knowledge about other units and should only talk to its immediate friends. The law is not a law in the literal sense but a guideline to reduce coupling between components in a system.

## Understanding LoD

### Definition:

- **Limited Knowledge**: Objects should only know about and interact with closely related objects rather than navigating an entire system.
  
### Purpose:

- **Reduce Coupling**: LoD aims to reduce dependencies in a system, which in turn reduces coupling, making the system more maintainable and adaptable.
- **Improve Modularity**: By encouraging less knowledge about the system, modules become more encapsulated and self-contained.

## Principles of LoD

### Talk to Friends:

- An object should only call methods on:
    - Itself
    - Objects passed in as a parameter
    - Any object it creates
    - Its direct components

### Don't Talk to Strangers:

- An object should avoid calling methods on objects returned from other calls.

### Method Chaining (Train Wrecks):

- Avoid method chaining (e.g., `this.getThat().getThose().doSomething()`), as it increases coupling between classes.

## Implementing LoD

### Encapsulation:

- Encapsulate complex logic within a class and provide a simple public interface.

### Delegation:

- If an object needs to perform an action that's outside its immediate scope, delegate the responsibility to a closely related object.

### Avoiding Deep Navigation:

- Refrain from reaching through objects to access methods or properties of other objects.

## Advanced Aspects of LoD

### LoD and Microservices:

- In a microservices architecture, services should know as little as possible about each other. They communicate through well-defined interfaces and avoid direct knowledge of each other's inner workings.

### LoD in Layered Architecture:

- In a layered architecture, a layer should only interact with its neighboring layers. This helps maintain clear boundaries and reduces dependencies across the system.

### LoD and API Design:

- Good API design often reflects the LoD, providing access to necessary functionality while hiding the underlying complexity.

## Python Example: Before and After Applying LoD

### Before Applying LoD:

```python
class Wallet:
    def __init__(self, amount):
        self.amount = amount

class Customer:
    def __init__(self, name, wallet):
        self.name = name
        self.wallet = wallet

    def check_funds(self, price):
        if self.wallet.amount >= price:
            return True
        else:
            return False

# Usage
wallet = Wallet(100)
customer = Customer("John", wallet)
print(customer.check_funds(50))  # Outputs: True
```

In this example, the `Customer` class violates LoD by directly accessing the `amount` in the `Wallet` class.

### After Applying LoD:

```python
class Wallet:
    def __init__(self, amount):
        self.amount = amount

    def has_funds(self, price):
        return self.amount >= price

class Customer:
    def __init__(self, name, wallet):
        self.name = name
        self.wallet = wallet

    def check_funds(self, price):
        return self.wallet.has_funds(price)

# Usage
wallet = Wallet(100)
customer = Customer("John", wallet)
print(customer.check_funds(50))  # Outputs: True
```

In this refactored example:

- The `Wallet` class now has a `has_funds` method, encapsulating the logic of checking funds.
- The `Customer` class now only interacts with the `Wallet` through the `has_funds` method, adhering to LoD.

See an extensive [[Real-Life Python Example for Law of Demeter (LoD) or Principle of Least Knowledge|example]].
## Conclusion

The Law of Demeter is a valuable principle in creating maintainable, loosely coupled, and modular software. It emphasizes that components should have limited knowledge and interaction with other components, promoting a design where objects are only aware of and interact with their immediate neighbors. While it can sometimes lead to more boilerplate code, the benefits of increased modularity, reduced coupling, and improved maintainability often outweigh these costs. As with all design principles, it should be applied judiciously and balanced with other considerations such as clarity and performance. Understanding and implementing the Law of Demeter can significantly contribute to the overall quality and robustness of a software system.
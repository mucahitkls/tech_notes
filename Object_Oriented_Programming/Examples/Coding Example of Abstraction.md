# JavaScript Example of Abstraction

## Scenario: Vehicle Control System

Imagine we're building a control system for various types of vehicles. Each vehicle has different ways to start, stop, and perform other actions.

### Without Abstraction:

Initially, we might directly interact with each vehicle type, leading to a complex and hard-to-manage system.

```javascript
class Car {
    startCar() {
        console.log("Car started with a key.");
    }

    stopCar() {
        console.log("Car stopped with brakes.");
    }
}

class Bicycle {
    startBicycle() {
        console.log("Bicycle started by pedaling.");
    }

    stopBicycle() {
        console.log("Bicycle stopped by pedaling backwards.");
    }
}

// Usage
let car = new Car();
let bicycle = new Bicycle();

car.startCar();
bicycle.startBicycle();
```

**Issues**: Directly managing each vehicle leads to a complex system, especially as more vehicle types are added. It's also not scalable or easy to maintain.

### After Introducing Abstraction:

We introduce an abstract `Vehicle` class that defines a standard interface for all vehicle actions. Each specific vehicle class then extends this base class and provides its own implementation.

```javascript
class Vehicle {
    start() {
        throw new Error("Method 'start()' must be implemented.");
    }

    stop() {
        throw new Error("Method 'stop()' must be implemented.");
    }
}

class Car extends Vehicle {
    start() {
        console.log("Car started with a key.");
    }

    stop() {
        console.log("Car stopped with brakes.");
    }
}

class Bicycle extends Vehicle {
    start() {
        console.log("Bicycle started by pedaling.");
    }

    stop() {
        console.log("Bicycle stopped by pedaling backwards.");
    }
}

function operateVehicle(vehicle) {
    vehicle.start();
    // other common vehicle operations...
    vehicle.stop();
}

// Usage
let car = new Car();
let bicycle = new Bicycle();

operateVehicle(car);
operateVehicle(bicycle);
```

**Improvements**: 
- **Simplicity**: The `operateVehicle` function can now interact with any vehicle type through a common interface, simplifying the overall system.
- **Scalability**: Adding new vehicle types is now easier and doesn't require changing how vehicles are operated.
- **Maintainability**: The system is now more organized and easier to understand.

# Python Example of Abstraction

## Scenario: Payment Processing System

Imagine we're building a system to process various types of payments like credit card, PayPal, or cryptocurrencies.

### Without Abstraction:

Initially, we might handle each payment type separately, leading to scattered and inflexible code.

```python
class CreditCardPayment:
    def process_credit_payment(self, amount):
        print(f"Processing credit card payment for: ${amount}")

class PayPalPayment:
    def process_paypal_payment(self, amount):
        print(f"Processing PayPal payment for: ${amount}")

# Usage
credit_payment = CreditCardPayment()
paypal_payment = PayPalPayment()

credit_payment.process_credit_payment(100)
paypal_payment.process_paypal_payment(150)
```

**Issues**: Managing each payment type separately is inefficient and inflexible, especially as more payment methods are added.

### After Introducing Abstraction:

We introduce an abstract `Payment` class that defines a standard interface for all payment actions. Each specific payment class then implements this interface.

```python
from abc import ABC, abstractmethod

class Payment(ABC):
    @abstractmethod
    def process_payment(self, amount):
        pass

class CreditCardPayment(Payment):
    def process_payment(self, amount):
        print(f"Processing credit card payment for: ${amount}")

class PayPalPayment(Payment):
    def process_payment(self, amount):
        print(f"Processing PayPal payment for: ${amount}")

def process_payment(payment, amount):
    payment.process_payment(amount)

# Usage
credit_payment = CreditCardPayment()
paypal_payment = PayPalPayment()

process_payment(credit_payment, 100)
process_payment(paypal_payment, 150)
```

**Improvements**: 
- **Consistency**: The `process_payment` function can now handle any payment type, providing a consistent way to process payments.
- **Extensibility**: Adding new payment methods is easier and requires minimal changes to the existing system.
- **Organized Code**: The system is more organized, and each payment type's specific logic is encapsulated within its class.

In both JavaScript and Python examples, introducing abstraction simplifies the system by providing a common interface for different types of objects. This makes the system more scalable, maintainable, and flexible, allowing for easier additions and modifications. Encapsulation is also applied here as the specific details of how each vehicle or payment type operates are hidden within their respective classes, exposing only the necessary interface to the outside world.
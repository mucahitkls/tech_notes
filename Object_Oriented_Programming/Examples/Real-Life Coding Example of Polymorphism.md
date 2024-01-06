
## Scenario: Payment System in an E-commerce Application

### Without Polymorphism:

Initially, we might handle different payment methods using separate functions or classes without a common interface, leading to code that's hard to manage and extend.

```javascript
class PayPalPayment {
    processPayment(amount) {
        console.log(`Processing PayPal payment for amount: $${amount}`);
        // PayPal specific payment logic
    }
}

class CreditCardPayment {
    processPayment(amount) {
        console.log(`Processing credit card payment for amount: $${amount}`);
        // Credit card specific payment logic
    }
}

// Usage
let paypalPayment = new PayPalPayment();
let creditCardPayment = new CreditCardPayment();

paypalPayment.processPayment(100);
creditCardPayment.processPayment(100);
```

**Issues**: Code scalability and maintainability are compromised. Introducing a new payment method requires significant changes and a new understanding of the system.

### After Introducing Polymorphism:

We introduce a base class `Payment` with a common interface for different payment methods. Each specific payment method class then extends this base class and provides its own implementation.

```javascript
class Payment {
    processPayment(amount) {
        throw new Error("Method 'processPayment()' must be implemented.");
    }
}

class PayPalPayment extends Payment {
    processPayment(amount) {
        console.log(`Processing PayPal payment for amount: $${amount}`);
        // PayPal specific payment logic
    }
}

class CreditCardPayment extends Payment {
    processPayment(amount) {
        console.log(`Processing credit card payment for amount: $${amount}`);
        // Credit card specific payment logic
    }
}

function processUserPayment(userPayment, amount) {
    userPayment.processPayment(amount);
}

// Usage
let paypalPayment = new PayPalPayment();
let creditCardPayment = new CreditCardPayment();

processUserPayment(paypalPayment, 100);
processUserPayment(creditCardPayment, 200);
```

**Improvements**: 
- Scalability: Adding a new payment method is now as simple as creating a new class that extends `Payment`.
- Flexibility: The `processUserPayment` function can process any payment type, demonstrating polymorphism.
- Maintainability: Changes to the payment processing for one method don't affect others.

# Python Example of Polymorphism

## Scenario: Shape Drawing Tool in a Graphics Application

### Without Polymorphism:

Initially, we might have separate functions or classes for each shape, leading to disjointed and inflexible code.

```python
class Circle:
    def draw_circle(self):
        print("Drawing a circle")

class Square:
    def draw_square(self):
        print("Drawing a square")

# Usage
circle = Circle()
square = Square()

circle.draw_circle()
square.draw_square()
```

**Issues**: Code is inflexible and hard to manage. Introducing a new shape requires understanding the whole system and adding unique methods.

### After Introducing Polymorphism:

We introduce a base class `Shape` with a common method `draw`. Each specific shape class then extends this base class and provides its own implementation.

```python
class Shape:
    def draw(self):
        raise NotImplementedError("Subclass must implement abstract method")

class Circle(Shape):
    def draw(self):
        print("Drawing a circle")

class Square(Shape):
    def draw(self):
        print("Drawing a square")

def draw_shape(shape):
    shape.draw()

# Usage
circle = Circle()
square = Square()

draw_shape(circle)
draw_shape(square)
```

**Improvements**: 
- Consistency: All shapes now share a common interface, `draw`, making it easier to interact with them polymorphically.
- Extensibility: Adding a new shape is as simple as creating a new class that extends `Shape` and implements the `draw` method.
- Simplicity: The `draw_shape` function can now handle any shape type, demonstrating polymorphism and reducing the need for conditional logic based on shape type.

In both JavaScript and Python examples, polymorphism significantly improved the design by making it more flexible, scalable, and maintainable. This allows for future enhancements and additions with minimal impact on existing code.
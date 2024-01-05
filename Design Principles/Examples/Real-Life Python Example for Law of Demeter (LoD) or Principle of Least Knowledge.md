
Consider a real-world application of a customer order processing system for an e-commerce platform. This system is responsible for handling customer orders, processing payments, and updating inventory. Initially, the system isn't designed with the Law of Demeter in mind, leading to high coupling and less maintainable code.

## Before Applying LoD

### Initial System:

- `CustomerOrder`: Manages the customer's order.
- `PaymentProcessor`: Handles the payment processing.
- `Inventory`: Manages inventory stock.

#### order_system.py (Before)

```python
class PaymentProcessor:
    def process_payment(self, customer, amount):
        print(f"Processing {amount} payment for {customer.name}")
        # Logic to process payment

class Inventory:
    def __init__(self):
        self.items = {'item1': 10, 'item2': 5}  # Simplified inventory

    def update_stock(self, item, quantity):
        if item in self.items:
            self.items[item] -= quantity
            print(f"Updated stock for {item}. Now: {self.items[item]}")

class CustomerOrder:
    def __init__(self, customer, item, quantity):
        self.customer = customer
        self.item = item
        self.quantity = quantity
        self.payment_processor = PaymentProcessor()
        self.inventory = Inventory()

    def process_order(self):
        self.payment_processor.process_payment(self.customer, 100)  # Simplified amount
        self.inventory.update_stock(self.item, self.quantity)

# Usage
customer = type('Customer', (object,), {'name': 'John Doe'})()
order = CustomerOrder(customer, 'item1', 2)
order.process_order()
```

In this non-LoD-compliant design, `CustomerOrder` directly interacts with internal details of both `PaymentProcessor` and `Inventory`.

## After Applying LoD

### Refactored System:

- Introduce intermediary methods to reduce direct interactions between classes.

#### order_system.py (After)

```python
class PaymentProcessor:
    def process_payment(self, customer_name, amount):
        print(f"Processing {amount} payment for {customer_name}")
        # Logic to process payment

class Inventory:
    def __init__(self):
        self.items = {'item1': 10, 'item2': 5}  # Simplified inventory

    def update_stock(self, item, quantity):
        if item in self.items:
            self.items[item] -= quantity
            print(f"Updated stock for {item}. Now: {self.items[item]}")

class CustomerOrder:
    def __init__(self, customer, item, quantity):
        self.customer_name = customer.name
        self.item = item
        self.quantity = quantity
        self.payment_processor = PaymentProcessor()
        self.inventory = Inventory()

    def process_order(self):
        self._process_payment(100)  # Simplified amount
        self._update_inventory()

    def _process_payment(self, amount):
        self.payment_processor.process_payment(self.customer_name, amount)

    def _update_inventory(self):
        self.inventory.update_stock(self.item, self.quantity)

# Usage
customer = type('Customer', (object,), {'name': 'John Doe'})()
order = CustomerOrder(customer, 'item1', 2)
order.process_order()
```

## Conclusion

In the refactored example:

- `CustomerOrder` no longer directly interacts with the internal details of `PaymentProcessor` and `Inventory`. Instead, it uses intermediary methods (`_process_payment` and `_update_inventory`) to handle these interactions.
- The system now adheres to the Law of Demeter by limiting the knowledge and interaction `CustomerOrder` has with other components.
- The refactored design is less coupled, making it easier to maintain and modify. Changes in `PaymentProcessor` or `Inventory` are less likely to require changes in `CustomerOrder`.

This LoD-compliant design enhances the system's maintainability and adaptability. It reduces the coupling between components, making the overall system more robust and easier to understand. The example emphasizes the benefits of adhering to the Law of Demeter or the Principle of Least Knowledge in a real-world application, leading to a cleaner, more efficient, and maintainable codebase.
# JavaScript Example of Message Passing

## Scenario: Customer Service System

Imagine we're building a customer service system where customers submit tickets and agents respond to them. The system notifies the customer when there's a response.

### Without Message Passing:

Initially, we might directly manipulate customer and agent data, leading to a rigid and tightly coupled system.

```javascript
class Customer {
    constructor(name) {
        this.name = name;
        this.tickets = [];
    }

    submitTicket(ticket) {
        this.tickets.push(ticket);
    }
}

class Agent {
    constructor(name) {
        this.name = name;
    }

    respondToTicket(customer, ticket, response) {
        const ticketIndex = customer.tickets.indexOf(ticket);
        if (ticketIndex !== -1) {
            customer.tickets[ticketIndex] = `${ticket} - Response: ${response}`;
        }
    }
}

// Usage
let customer = new Customer("John Doe");
let agent = new Agent("Agent Smith");

customer.submitTicket("Issue with my order.");
agent.respondToTicket(customer, "Issue with my order.", "We are on it!");

console.log(customer.tickets);  // ['Issue with my order. - Response: We are on it!']
```

**Issues**: The `Agent` class directly manipulates the `Customer`'s tickets. This design is not flexible and violates encapsulation principles.

### After Introducing Message Passing:

We introduce a method for message passing, allowing `Agent` and `Customer` to communicate without directly accessing each other's data.

```javascript
class Customer {
    constructor(name) {
        this.name = name;
        this.tickets = [];
    }

    submitTicket(ticket) {
        this.tickets.push(ticket);
    }

    receiveResponse(ticket, response) {
        const ticketIndex = this.tickets.indexOf(ticket);
        if (ticketIndex !== -1) {
            this.tickets[ticketIndex] = `${ticket} - Response: ${response}`;
        }
    }
}

class Agent {
    constructor(name) {
        this.name = name;
    }

    respondToTicket(customer, ticket, response) {
        customer.receiveResponse(ticket, response);
    }
}

// Usage
let customer = new Customer("John Doe");
let agent = new Agent("Agent Smith");

customer.submitTicket("Issue with my order.");
agent.respondToTicket(customer, "Issue with my order.", "We are on it!");

console.log(customer.tickets);  // ['Issue with my order. - Response: We are on it!']
```

**Improvements**: 
- **Encapsulation**: `Customer` and `Agent` now adhere to encapsulation. `Agent` doesn't directly manipulate `Customer`'s tickets.
- **Flexibility**: The system is more flexible. If the ticketing system changes, we only need to update the `Customer`'s `receiveResponse` method.
- **Modularity**: Each class is now more focused on its responsibilities.

# Python Example of Message Passing

## Scenario: Online Food Ordering System

We're creating a system where customers can order food from restaurants. Restaurants need to receive and confirm orders.

### Without Message Passing:

Initially, the restaurant might directly access and modify the customer's order details.

```python
class Customer:
    def __init__(self, name):
        self.name = name
        self.orders = []

    def placeOrder(self, restaurant, order):
        self.orders.append(order)
        restaurant.receiveOrder(order)

class Restaurant:
    def __init__(self, name):
        self.name = name

    def receiveOrder(self, order):
        print(f"Order received for {order}.")

# Usage
customer = Customer("Jane Doe")
restaurant = Restaurant("Good Food")

customer.placeOrder(restaurant, "Pizza Margherita")
```

**Issues**: The `Restaurant` class directly interacts with the `Customer`'s orders. This design is not flexible and can lead to issues if the order handling process changes.

### After Introducing Message Passing:

We introduce methods for message passing, allowing the `Customer` and `Restaurant` to communicate more flexibly.

```python
class Customer:
    def __init__(self, name):
        self.name = name
        self.orders = []

    def placeOrder(self, restaurant, order):
        self.orders.append(order)
        restaurant.receiveOrder(order, self)

    def confirmOrder(self, order):
        print(f"Order confirmed for {order}.")

class Restaurant:
    def __init__(self, name):
        self.name = name

    def receiveOrder(self, order, customer):
        print(f"Order received for {order}.")
        customer.confirmOrder(order)

# Usage
customer = Customer("Jane Doe")
restaurant = Restaurant("Good Food")

customer.placeOrder(restaurant, "Pizza Margherita")
```

**Improvements**: 
- **Encapsulation**: Both `Customer` and `Restaurant` now encapsulate their data and expose only what's necessary.
- **Flexibility**: Changes to order processing can be made within the `Restaurant` or `Customer` without affecting the other.
- **Clear Communication**: The system now has a clear communication path, enhancing readability and maintainability.

In both JavaScript and Python examples, introducing message passing transformed the system, providing a clear communication mechanism between objects and adhering to encapsulation principles. This approach not only makes the system more robust and maintainable but also provides a solid foundation for future enhancements and scalability.
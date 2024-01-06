
## JavaScript Example: Notification Service

### Scenario:

Imagine you're creating a notification service for an application that can send various types of notifications, such as Email, SMS, and Push notifications. Initially, you might directly instantiate notification classes throughout your code.

#### Without Factory Method:

You create and use notification objects directly, leading to tightly coupled and inflexible code.

```javascript
class EmailNotification {
    send(message) {
        console.log(`Sending Email: ${message}`);
    }
}

class SMSNotification {
    send(message) {
        console.log(`Sending SMS: ${message}`);
    }
}

class PushNotification {
    send(message) {
        console.log(`Sending Push Notification: ${message}`);
    }
}

// Usage
const email = new EmailNotification();
email.send("Hello, Email!");

const sms = new SMSNotification();
sms.send("Hello, SMS!");

// As new notification types are added, the code grows more complex and harder to manage.
```

**Issues**: This approach is inflexible and doesn't scale well. Introducing a new notification type requires changes throughout your code.

#### With Factory Method:

Refactor the system to use a Factory Method pattern, which provides a way to create objects without specifying the exact class.

```javascript
// Creator
class NotificationFactory {
    createNotification(type) {
        switch (type) {
            case 'email':
                return new EmailNotification();
            case 'sms':
                return new SMSNotification();
            case 'push':
                return new PushNotification();
            default:
                throw new Error('Notification type not supported.');
        }
    }
}

// Concrete Products
class EmailNotification {
    send(message) {
        console.log(`Sending Email: ${message}`);
    }
}

class SMSNotification {
    send(message) {
        console.log(`Sending SMS: ${message}`);
    }
}

class PushNotification {
    send(message) {
        console.log(`Sending Push Notification: ${message}`);
    }
}

// Usage
const factory = new NotificationFactory();
const email = factory.createNotification('email');
email.send('Hello, Email!');

const sms = factory.createNotification('sms');
sms.send('Hello, SMS!');

// Adding a new notification type is now easier and doesn't require changes to the client code.
```

**Improvements**: 
- **[[Flexibility in Software Development|Flexibility]]**: New notification types can be added without altering the client code.
- **[[Decoupling in Software Development|Decoupling]]**: The client is decoupled from the concrete classes and relies on the factory for object creation.
- **[[Scalability in Software Development|Scalability]]**: The code is more maintainable and scalable as new notification types are introduced.

## Python Example: Transport Service

### Scenario:

Imagine you're building a transport service in a logistics application. The service can handle various types of transportation like Road, Sea, and Air. Initially, you might instantiate transport classes directly.

#### Without Factory Method:

Directly creating instances of transport classes results in inflexible and tightly coupled code.

```python
class RoadTransport:
    def deliver(self):
        print("Delivering by road.")

class SeaTransport:
    def deliver(self):
        print("Delivering by sea.")

class AirTransport:
    def deliver(self):
        print("Delivering by air.")

# Usage
road = RoadTransport()
road.deliver()

sea = SeaTransport()
sea.deliver()

# Adding a new transport type leads to further complexity.
```

**Issues**: Similar to the JavaScript example, this approach doesn't scale well and leads to code duplication.

#### With Factory Method:

Implement the Factory Method pattern to provide a flexible way to create transport objects.

```python
# Creator
class TransportFactory:
    def get_transport(self, type):
        if type == 'road':
            return RoadTransport()
        elif type == 'sea':
            return SeaTransport()
        elif type == 'air':
            return AirTransport()
        else:
            raise ValueError(f"Transport type {type} is not supported.")

# Concrete Products
class RoadTransport:
    def deliver(self):
        print("Delivering by road.")

class SeaTransport:
    def deliver(self):
        print("Delivering by sea.")

class AirTransport:
    def deliver(self):
        print("Delivering by air.")

# Usage
factory = TransportFactory()
road = factory.get_transport('road')
road.deliver()

sea = factory.get_transport('sea')
sea.deliver()

# Adding a new transport type is now easier and requires minimal changes.
```

**Improvements**: 
- **[[Flexibility in Software Development|Flexibility]]**: You can add new transport types without altering existing client code.
- **[[Decoupling in Software Development|Decoupling]]**: The client doesn't need to know the details of object creation, leading to cleaner and more maintainable code.
- **[[Scalability in Software Development|Scalability]]**: The system becomes more scalable and easier to extend with new types of transportation.

In both examples, the Factory Method pattern significantly improved the design of the system by providing a flexible and scalable way to create objects. It decouples the client code from the concrete classes and adheres to the open/closed principle, making the system more robust and easier to maintain. This pattern is especially useful in systems where the types of objects to create can vary and might be extended in the future.
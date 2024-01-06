
## JavaScript Example: Integrating Third-Party Libraries

### Scenario: 
Imagine you're working on a web application that needs to display maps. Initially, you chose a specific mapping service, like Google Maps. Over time, you decide to switch to a different provider, such as Leaflet, due to better features or pricing. However, you don't want to refactor your entire application. This is where the Adapter pattern comes in handy.

#### Without Adapter:
You might have a tightly coupled system where your application directly calls the Google Maps API.

```javascript
class GoogleMap {
    show(location) {
        console.log(`GoogleMap: Centering at ${location}`);
        // Google Maps specific logic to render the map
    }
}

// Usage
const map = new GoogleMap();
map.show('New York');  // Directly using Google Maps
```

**Issues**: Switching to a different map provider requires substantial changes throughout your code, leading to potential bugs and a lot of refactoring.

#### With Adapter:

Firstly, you create an interface that your application will use to interact with the map, and then you implement adapters for each map provider.

```javascript
// Map interface
class MapInterface {
    show(location) {}
}

// Google Maps Adapter
class GoogleMapAdapter extends MapInterface {
    constructor() {
        super();
        this.googleMap = new GoogleMap();
    }

    show(location) {
        this.googleMap.show(location);
    }
}

// New Map Provider (Leaflet) Adapter
class LeafletMapAdapter extends MapInterface {
    constructor() {
        super();
        this.leafletMap = new LeafletMap();
    }

    show(location) {
        this.leafletMap.display(location);  // Assuming Leaflet uses 'display' method
    }
}

// Google Maps Specific Class
class GoogleMap {
    show(location) {
        console.log(`GoogleMap: Centering at ${location}`);
        // Specific logic to render Google Map
    }
}

// Leaflet Specific Class
class LeafletMap {
    display(location) {
        console.log(`LeafletMap: Displaying at ${location}`);
        // Specific logic to render Leaflet Map
    }
}

// Usage
let map = new GoogleMapAdapter();
map.show('New York');  // Using Google Maps

map = new LeafletMapAdapter();
map.show('London');    // Switching to Leaflet without major code changes
```

**Improvements**: 
- **[[Decoupling in Software Development|Decoupling]]**: Your application is now decoupled from specific map providers. It interacts with a consistent interface, `MapInterface`.
- **Flexibility**: Switching map providers or adding new ones doesn't require significant changes in your application's code. You just need to introduce a new adapter.
- **Maintainability**: With the application depending on a stable interface, the overall codebase becomes more maintainable and robust against changes in third-party services.

## Python Example: Payment Processing System

### Scenario:
Your e-commerce platform needs to process payments. Initially, you integrated Stripe for payment processing. As your business grows, you decide to add PayPal as another payment option to provide more flexibility to your users.

#### Without Adapter:
You might have direct integration with the Stripe API in your order processing system.

```python
class StripePayment:
    def make_payment(self, amount):
        print(f"Stripe: Processing ${amount} payment.")

# Usage
class Order:
    def __init__(self, payment_processor):
        self.payment_processor = payment_processor

    def checkout(self, amount):
        self.payment_processor.make_payment(amount)

# Directly using Stripe
stripe_payment = StripePayment()
order = Order(stripe_payment)
order.checkout(100)
```

**Issues**: If you want to add PayPal as an option, you'll need to modify the `Order` class and everywhere else you've used `StripePayment`.

#### With Adapter:

Create a common payment interface and adapters for each payment processor.

```python
# Payment Interface
class PaymentProcessor:
    def pay(self, amount):
        pass

# Stripe Adapter
class StripeAdapter(PaymentProcessor):
    def __init__(self):
        self.stripe = StripePayment()

    def pay(self, amount):
        self.stripe.make_payment(amount)

# New PayPal Adapter
class PayPalAdapter(PaymentProcessor):
    def __init__(self):
        self.paypal = PayPalPayment()

    def pay(self, amount):
        self.paypal.process_payment(amount)

# Stripe Specific Class
class StripePayment:
    def make_payment(self, amount):
        print(f"Stripe: Processing ${amount} payment.")

# PayPal Specific Class
class PayPalPayment:
    def process_payment(self, amount):
        print(f"PayPal: Processing ${amount} payment.")

# Usage
class Order:
    def __init__(self, payment_processor):
        self.payment_processor = payment_processor

    def checkout(self, amount):
        self.payment_processor.pay(amount)

# Using Stripe
stripe_adapter = StripeAdapter()
order = Order(stripe_adapter)
order.checkout(100)

# Switching to PayPal with minimal code change
paypal_adapter = PayPalAdapter()
order = Order(paypal_adapter)
order.checkout(150)
```

**Improvements**: 
- **Decoupling**: The order processing system is decoupled from specific payment processors. It interacts with a consistent payment interface.
- **Flexibility**: Adding a new payment option like PayPal doesn't require changing the order processing logic.
- **Scalability**: The system can easily scale to include more payment processors as your business grows.

In both examples, the Adapter pattern allowed the applications to adapt

 to new requirements and third-party services with minimal changes and disruptions. This pattern is incredibly powerful when you're dealing with external libraries, APIs, or systems that are outside of your control, or when you anticipate future changes to external systems. It ensures your application remains flexible, maintainable, and robust against external changes.
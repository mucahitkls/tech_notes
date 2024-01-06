
## JavaScript Example: E-commerce Shipping Cost Calculator

### Scenario:

Imagine you're developing an e-commerce platform. Different types of products require different shipping strategies due to their size, weight, or value. Without the Strategy pattern, you might have a complex conditional system to handle this, which becomes unwieldy as more strategies are added.

#### Without Strategy:

You implement shipping cost calculation directly within the order class, leading to a rigid and hard-to-manage system.

```javascript
class Order {
    constructor(productType, value) {
        this.productType = productType;
        this.value = value;
    }

    calculateShipping() {
        if (this.productType === 'small') {
            return this.value * 0.1;
        } else if (this.productType === 'large') {
            return this.value * 0.2 + 10;
        }
        // More conditionals for other product types
    }
}

// Usage
const smallOrder = new Order('small', 100);
console.log(smallOrder.calculateShipping());  // Shipping cost for a small item
```

**Issues**: The `Order` class handles all the shipping logic, violating the single responsibility principle. Adding new shipping strategies requires modifying the `Order` class.

#### With Strategy:

Refactor the system to use the Strategy pattern, decoupling the order and shipping cost calculation.

```javascript
// Strategy Interface
class ShippingStrategy {
    calculate(orderValue) {}
}

// Concrete Strategies
class SmallItemShipping extends ShippingStrategy {
    calculate(orderValue) {
        return orderValue * 0.1;
    }
}

class LargeItemShipping extends ShippingStrategy {
    calculate(orderValue) {
        return orderValue * 0.2 + 10;
    }
}

// Context
class Order {
    constructor(strategy, value) {
        this.strategy = strategy;
        this.value = value;
    }

    calculateShipping() {
        return this.strategy.calculate(this.value);
    }
}

// Usage
const smallStrategy = new SmallItemShipping();
const smallOrder = new Order(smallStrategy, 100);
console.log(smallOrder.calculateShipping());  // Shipping cost for a small item

const largeStrategy = new LargeItemShipping();
const largeOrder = new Order(largeStrategy, 100);
console.log(largeOrder.calculateShipping());  // Shipping cost for a large item
```

**Improvements**: 
- **[[Flexibility in Software Development|Flexibility]]**: Easily switch or add new shipping strategies without modifying the `Order` class.
- **[[Decoupling in Software Development|Decoupling]]**: Shipping logic is decoupled from the order logic, adhering to the single responsibility principle.
- **[[Maintainability in Software Development|Maintainability]]**: Each shipping strategy can be maintained independently, improving maintainability.

## Python Example: Content Rendering

### Scenario:

Imagine you're building a content management system where different types of content need to be rendered in various formats (e.g., HTML, Markdown, Plain Text). Without the Strategy pattern, you might implement rendering methods directly within the content class.

#### Without Strategy:

Direct rendering methods within the content class lead to a tightly coupled and inflexible design.

```python
class Content:
    def __init__(self, text):
        self.text = text

    def render(self, formatType):
        if formatType == 'html':
            return f"<html>{self.text}</html>"
        elif formatType == 'markdown':
            return f"**{self.text}**"
        # More conditionals for other formats

# Usage
content = Content("Hello, World!")
print(content.render('html'))  # HTML rendering
```

**Issues**: The `Content` class is responsible for all rendering logic. Adding new rendering formats requires modifying the `Content` class.

#### With Strategy:

Implement the Strategy pattern to decouple content and rendering logic.

```python
# Strategy Interface
class RenderStrategy:
    def render(self, text):
        pass

# Concrete Strategies
class HtmlRenderer(RenderStrategy):
    def render(self, text):
        return f"<html>{text}</html>"

class MarkdownRenderer(RenderStrategy):
    def render(self, text):
        return f"**{text}**"

# Context
class Content:
    def __init__(self, strategy, text):
        self.strategy = strategy
        self.text = text

    def render(self):
        return self.strategy.render(self.text)

# Usage
htmlStrategy = HtmlRenderer()
content = Content(htmlStrategy, "Hello, World!")
print(content.render())  # HTML rendering

markdownStrategy = MarkdownRenderer()
content = Content(markdownStrategy, "Hello, World!")
print(content.render())  # Markdown rendering
```

**Improvements**: 
- **[[Flexibility in Software Development|Flexibility]]**: Easily switch or add new rendering strategies without modifying the `Content` class.
- **[[Decoupling in Software Development|Decoupling]]**: Rendering logic is decoupled from the content logic, adhering to the single responsibility principle.
- **[[Scalability in Software Development|Scalability]]**: The system can easily accommodate new rendering formats as needed.

In both examples, the Strategy pattern significantly improved the design of the system by providing a flexible and scalable way to introduce different behaviors (shipping strategies, rendering formats). It decouples the context (Order, Content) from the strategies, adhering to the open/closed principle and making the system more robust and easier to maintain. This pattern is especially useful in systems where multiple algorithms or behaviors might be required, enabling smooth operation and better adaptability to future changes.
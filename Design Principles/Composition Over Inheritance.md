
## Introduction to Composition Over Inheritance

Composition Over Inheritance is a principle that encourages the use of composition (combining simple objects to build more complex ones) rather than inheritance (forming hierarchical relationships between classes) to achieve polymorphic behavior and code reuse. This principle is a fundamental concept in object-oriented design and is particularly relevant when considering the flexibility and maintainability of a system.

## Understanding Composition Over Inheritance

### Definition:

- **Composition**: An object contains other objects as part of its state. Each object is responsible for its functionality. The containing object delegates tasks to its components.
- **Inheritance**: An object inherits behavior and properties from a parent object. It's a hierarchical relationship where the child class extends the parent class.

### Purpose:

- **Flexibility**: Composition allows for more flexible design as it's easier to change the behavior of a system composed of several loosely coupled objects.
- **Avoiding the Fragile Base Class Problem**: Changes in base classes can inadvertently affect all subclasses, potentially causing bugs.
- **Reducing Complexity**: Inheritance can lead to deep hierarchies that are difficult to understand and maintain.

## Principles of Composition Over Inheritance

### Favoring Loose Coupling:

- Objects should be loosely coupled. They interact with each other through interfaces or abstract classes, not concrete implementations.

### Encapsulation:

- Components encapsulate their behavior and state, exposing only what is necessary through well-defined interfaces.

### Single Responsibility:

- Each class or component should have a single responsibility and reason to change, aligning with the Single Responsibility Principle (SRP).

## Implementing Composition Over Inheritance

### Identifying Components:

- Break down the system into smaller, manageable components that encapsulate specific functionality.

### Designing Interfaces:

- Define interfaces or abstract classes that specify what behaviors components should have.

### Composing Objects:

- Build complex objects by combining simpler, focused objects.

## Advanced Aspects of Composition Over Inheritance

### Design Patterns:

- Several design patterns such as Strategy, Decorator, and Composite leverage composition to provide flexibility and reusability.

### Refactoring Inheritance Hierarchies:

- Existing inheritance hierarchies can often be refactored to use composition, improving their maintainability and adaptability.

### Testing:

- Composition can lead to easier testing, as components can be tested in isolation and mocks/stubs can replace them in tests.

## Python Example: Before and After Applying Composition Over Inheritance

### Before Applying Composition Over Inheritance:

```python
# Inheritance-based approach
class Bird:
    def __init__(self, name):
        self.name = name

    def fly(self):
        print(f"{self.name} is flying")

class Penguin(Bird):
    def fly(self):
        raise NotImplementedError("Penguins can't fly")

# Usage
penguin = Penguin("Pengu")
penguin.fly()  # Raises NotImplementedError
```

In this example, `Penguin` inherits from `Bird`, but it has to override the `fly` method because penguins can't fly, violating the Liskov Substitution Principle.

### After Applying Composition Over Inheritance:

```python
# Composition-based approach
class FlyingBehavior:
    def fly(self):
        print("Flying")

class NoFlyingBehavior:
    def fly(self):
        print("Can't fly")

class Bird:
    def __init__(self, name, flying_behavior):
        self.name = name
        self.flying_behavior = flying_behavior

    def fly(self):
        self.flying_behavior.fly()

# Usage
flying_bird = Bird("Sparrow", FlyingBehavior())
flying_bird.fly()  # Outputs: Flying

non_flying_bird = Bird("Penguin", NoFlyingBehavior())
non_flying_bird.fly()  # Outputs: Can't fly
```

In this refactored example:

- `Bird` no longer has a `fly` method by default. Instead, it delegates the flying behavior to a `FlyingBehavior` object.
- `FlyingBehavior` and `NoFlyingBehavior` encapsulate the flying logic.
- This design adheres to the Composition Over Inheritance principle by favoring composition to provide the needed functionality.

See an extensive [[Real-Life Python Example for Composition Over Inheritance|example]].
## Conclusion

Composition Over Inheritance is a powerful principle in object-oriented design that encourages building more flexible, maintainable, and understandable systems. By favoring composition, developers can create systems that are easier to modify and extend, avoiding many of the pitfalls associated with deep and rigid inheritance hierarchies. This principle aligns with other fundamental design principles, promoting better software architecture and design patterns. As with all principles, it should be applied judiciously and in balance with other considerations, always aiming for the clearest and most effective solution to the problem at hand.
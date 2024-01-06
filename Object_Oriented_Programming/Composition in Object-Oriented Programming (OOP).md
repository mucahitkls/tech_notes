
## Introduction to Composition in OOP

Composition is a fundamental concept in object-oriented programming that deals with building complex objects from simpler ones. It's a 'has-a' relationship where a complex object 'has' one or more objects of another class as its parts.

## Understanding Composition in OOP

### Definition:

- **Part-Whole Relationship**: Composition is a way to combine simple objects or data types into more complex ones by including them as parts of a new object.

### Purpose:

- **Flexibility and Reusability**: It provides a means to structure and organize code in a modular fashion, promoting reusability and flexibility.

## Key Concepts of Composition in OOP

### 1. Component Objects:

- The objects that are used as parts of a more complex object.

### 2. Composite Object:

- The complex object that is composed of one or more component objects.

### 3. Lifespan Dependency:

- The component objects' lifespan is tied to the composite object. If the composite object is destroyed, its components are also destroyed.

### 4. Strong Relationship:

- Composition implies a strong relationship where the composite object has full responsibility for its components.

## Advanced Aspects of Composition in OOP

### 1. Composition Over Inheritance:

- Favoring composition over inheritance as a way of combining simple objects into complex ones without creating rigid class hierarchies.

### 2. Loose Coupling:

- Composition allows for loose coupling between classes, making the system more flexible and easier to change.

### 3. Dynamic Composition:

- Objects can be composed at runtime, providing greater flexibility and adaptability.

### 4. Encapsulation:

- Encapsulating the composed objects within the composite object, exposing only what's necessary through an interface.

## JavaScript Composition Example

### Before Composition:

Without composition, you might create a monolithic class that is inflexible and difficult to manage.

```javascript
class Smartphone {
    constructor(brand, camera, processor) {
        this.brand = brand;
        this.camera = camera;
        this.processor = processor;
    }

    takePicture() {
        console.log(`Taking a picture with a ${this.camera} camera.`);
    }

    runApplication() {
        console.log(`Running an application on a ${this.processor} processor.`);
    }
}

// Using the class
let myPhone = new Smartphone("BrandX", "12MP", "X1");
myPhone.takePicture();
myPhone.runApplication();
```

### After Composition:

With composition, you create separate classes and then compose them into a complex object.

```javascript
class Camera {
    constructor(type) {
        this.type = type;
    }

    takePicture() {
        console.log(`Taking a picture with a ${this.type} camera.`);
    }
}

class Processor {
    constructor(type) {
        this.type = type;
    }

    runApplication() {
        console.log(`Running an application on a ${this.type} processor.`);
    }
}

class Smartphone {
    constructor(brand, camera, processor) {
        this.brand = brand;
        this.camera = new Camera(camera);
        this.processor = new Processor(processor);
    }
}

// Using the composed class
let myPhone = new Smartphone("BrandX", "12MP", "X1");
myPhone.camera.takePicture();
myPhone.processor.runApplication();
```

## Python Composition Example

### Before Composition:

In a non-composite structure, functionalities are bundled into a single class.

```python
class Smartphone:
    def __init__(self, brand, camera, processor):
        self.brand = brand
        self.camera = camera
        self.processor = processor

    def take_picture(self):
        print(f"Taking a picture with a {self.camera} camera.")

    def run_application(self):
        print(f"Running an application on a {self.processor} processor.")

# Using the class
my_phone = Smartphone("BrandX", "12MP", "X1")
my_phone.take_picture()
my_phone.run_application()
```

### After Composition:

Composition allows for a modular approach where each part is a separate class.

```python
class Camera:
    def __init__(self, type):
        self.type = type

    def take_picture(self):
        print(f"Taking a picture with a {self.type} camera.")

class Processor:
    def __init__(self, type):
        self.type = type

    def run_application(self):
        print(f"Running an application on a {self.type} processor.")

class Smartphone:
    def __init__(self, brand, camera, processor):
        self.brand = brand
        self.camera = Camera(camera)
        self.processor = Processor(processor)

# Using the composed class
my_phone = Smartphone("BrandX", "12MP", "X1")
my_phone.camera.take_picture()
my_phone.processor.run_application()
```


See an extensive [[Coding Example of Composition|example]].

## Conclusion

Composition in OOP is a powerful design principle that promotes code reusability, flexibility, and maintainability. It allows for the creation of complex objects by combining simpler, independent objects. Understanding and effectively implementing composition can lead to a more modular and adaptable codebase, making it easier to manage and evolve. As you continue to explore and apply composition, you'll find it an invaluable tool in your OOP toolkit, enabling you to design and implement complex systems more efficiently and effectively. In languages like JavaScript and Python, composition provides a clear and structured way to assemble complex functionalities, bringing the benefits of modularity and reusability to your code.
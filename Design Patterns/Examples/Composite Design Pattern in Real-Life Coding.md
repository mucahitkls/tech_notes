## JavaScript Example: File System Structure

### Scenario:

Imagine you're creating a file system representation in a file manager application. The file system consists of both files and folders, where folders can contain other files and folders. Without the Composite pattern, you might treat files and folders as separate entities, leading to complex and duplicated code when performing operations like calculating total size or searching files.

#### Without Composite:

Initially, you might have distinct classes for files and folders with repetitive code for similar operations.

```javascript
class File {
    constructor(name, size) {
        this.name = name;
        this.size = size;
    }

    getSize() {
        return this.size;
    }
}

class Folder {
    constructor(name) {
        this.name = name;
        this.children = [];
    }

    add(child) {
        this.children.push(child);
    }

    getSize() {
        let total = 0;
        this.children.forEach(child => {
            if (child instanceof File) {
                total += child.getSize();
            } else if (child instanceof Folder) {
                total += child.getSize();
            }
        });
        return total;
    }
}

// Usage
const file1 = new File("file1.txt", 10);
const file2 = new File("file2.txt", 20);
const folder = new Folder("folder");
folder.add(file1);
folder.add(file2);

console.log(folder.getSize());  // 30 - Cumulative size of all files in the folder
```

**Issues**: The code for calculating the size is duplicated and becomes more complex as you add more operations. Managing the hierarchy is also complex and error-prone.

#### With Composite:

Refactor the system so that both files and folders share the same interface. Treat both files and folders as 'FileComponents'.

```javascript
// Component Interface
class FileComponent {
    getSize() {}
}

// Leaf
class File extends FileComponent {
    constructor(name, size) {
        super();
        this.name = name;
        this.size = size;
    }

    getSize() {
        return this.size;
    }
}

// Composite
class Folder extends FileComponent {
    constructor(name) {
        super();
        this.name = name;
        this.children = [];
    }

    add(child) {
        this.children.push(child);
    }

    getSize() {
        return this.children.reduce((total, child) => total + child.getSize(), 0);
    }
}

// Usage
const file1 = new File("file1.txt", 10);
const file2 = new File("file2.txt", 20);
const folder = new Folder("folder");
folder.add(file1);
folder.add(file2);

console.log(folder.getSize());  // 30 - Cumulative size of all files in the folder
```

**Improvements**: 
- **Unified Interface**: Files and folders are treated uniformly. Operations like `getSize()` are part of a common interface.
- **Simplified Hierarchy Management**: Adding or removing children is now straightforward and less error-prone.
- **Scalability**: Introducing new operations or components doesn't require changing existing code, adhering to the open/closed principle.

## Python Example: Menu System

### Scenario:

Imagine you're building a menu system for a restaurant application. The menu consists of individual items and sub-menus, where each sub-menu can contain other items and sub-menus. Without the Composite pattern, you might create separate classes for items and menus, leading to complex code for operations like printing the entire menu.

#### Without Composite:

Initially, you might have separate handling for items and sub-menus.

```python
class MenuItem:
    def __init__(self, name, price):
        self.name = name
        self.price = price

    def print(self):
        print(f"{self.name}: ${self.price}")

class Menu:
    def __init__(self, name):
        self.name = name
        self.children = []

    def add(self, child):
        self.children.append(child)

    def print(self):
        print(self.name)
        for child in self.children:
            if isinstance(child, MenuItem):
                child.print()
            elif isinstance(child, Menu):
                child.print()

# Usage
main_menu = Menu("Main Menu")
sub_menu = Menu("Desserts")
item1 = MenuItem("Cake", 5)
item2 = MenuItem("Ice Cream", 4)

sub_menu.add(item1)
sub_menu.add(item2)
main_menu.add(sub_menu)

main_menu.print()
```

**Issues**: Managing and performing operations across different types of menu components is complex and repetitive.

#### With Composite:

Refactor so that both menu items and sub-menus share the same interface.

```python
# Component Interface
class MenuComponent:
    def print(self):
        pass

# Leaf
class MenuItem(MenuComponent):
    def __init__(self, name, price):
        self.name = name
        self.price = price

    def print(self):
        print(f"{self.name}: ${self.price}")

# Composite
class Menu(MenuComponent):
    def __init__(self, name):
        self.name = name
        self.children = []

    def add(self, child):
        self.children.append(child)

    def print(self):
        print(self.name)
        for child in self.children:
            child.print()

# Usage
main_menu = Menu("Main Menu")
sub_menu = Menu("Desserts")
item1 = MenuItem("Cake", 5)
item2 = MenuItem("Ice Cream", 4)

sub_menu.add(item1)
sub_menu.add(item2)
main_menu.add(sub_menu)

main_menu.print()
```

**Improvements**: 
- **Unified Interface**: Menu items and sub-menus are treated uniformly. Both types of objects implement a common interface.
- **Simplified Hierarchy Management**: Adding or removing items or sub-menus is now straightforward.
- **Scalability**: Introducing new types of menu components or operations is easier and doesn't require extensive refactoring.

In both examples, the Composite pattern simplified the management of hierarchical structures and allowed treating individual objects and compositions uniformly. This pattern is especially beneficial in scenarios where you're dealing with a tree-like structure and want to perform operations on all elements of the

 structure, whether they're composite or leaf nodes. The resulting code is more maintainable, scalable, and easier to understand.
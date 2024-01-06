
## JavaScript Example: Home Automation System

### Scenario:

Imagine you're developing a smart home automation system where users can control various devices like lights, fans, and thermostats. Initially, you might directly invoke actions on these devices. As the system grows, you want a more scalable and flexible way to handle user commands, including undoing actions or queuing up a series of actions.

#### Without Command:

Direct interaction with device classes, leading to a rigid and less manageable system.

```javascript
class Light {
    on() {
        console.log("Light turned on");
    }
    off() {
        console.log("Light turned off");
    }
}

class Fan {
    on() {
        console.log("Fan turned on");
    }
    off() {
        console.log("Fan turned off");
    }
}

// Usage
const light = new Light();
const fan = new Fan();

light.on();
fan.on();
```

**Issues**: Directly managing devices leads to tight coupling and difficulty managing complex sequences or undoing actions.

#### With Command:

Implement the Command pattern to encapsulate actions as objects, allowing for more flexible control over user actions.

```javascript
// Command Interface
class Command {
    execute() {}
    undo() {}
}

// Concrete Commands
class LightOnCommand extends Command {
    constructor(light) {
        super();
        this.light = light;
    }
    execute() {
        this.light.on();
    }
    undo() {
        this.light.off();
    }
}

class FanOnCommand extends Command {
    constructor(fan) {
        super();
        this.fan = fan;
    }
    execute() {
        this.fan.on();
    }
    undo() {
        this.fan.off();
    }
}

// Receiver
class Light {
    on() {
        console.log("Light turned on");
    }
    off() {
        console.log("Light turned off");
    }
}

class Fan {
    on() {
        console.log("Fan turned on");
    }
    off() {
        console.log("Fan turned off");
    }
}

// Invoker
class RemoteControl {
    submit(command) {
        command.execute();
    }

    revert(command) {
        command.undo();
    }
}

// Usage
const light = new Light();
const fan = new Fan();

const lightOn = new LightOnCommand(light);
const fanOn = new FanOnCommand(fan);

const remote = new RemoteControl();
remote.submit(lightOn);  // Light turned on
remote.submit(fanOn);    // Fan turned on

remote.revert(lightOn);  // Light turned off
remote.revert(fanOn);    // Fan turned off
```

**Improvements**: 
- **[[Decoupling in Software Development|Decoupling]]**: The invoker (`RemoteControl`) is decoupled from the receivers (`Light`, `Fan`). It only knows about the command interface.
- **[[Flexibility in Software Development|Flexibility]]**: Easy to add new commands or change the order of execution.
- **Undo functionality**: You can easily add undo functionality for commands.

## Python Example: Text Editor Actions

### Scenario:

Imagine you're building a text editor. Users can perform actions like writing, deleting, copying, and pasting text. Initially, you might directly perform these actions. As the editor grows more complex, you want a flexible way to handle user actions, including undoing and redoing them.

#### Without Command:

Directly performing text actions, leading to tightly coupled and less manageable code.

```python
class TextEditor:
    def __init__(self):
        self.text = ""

    def write(self, words):
        self.text += words

    def delete(self):
        self.text = self.text[:-1]

    def copy(self):
        self.clipboard = self.text

    def paste(self):
        self.text += self.clipboard

# Usage
editor = TextEditor()
editor.write("Hello, ")
editor.write("World!")
editor.delete()
```

**Issues**: Directly managing actions makes it difficult to implement complex features like undo/redo and macros.

#### With Command:

Use the Command pattern to encapsulate text actions, allowing for more flexible and complex action management.

```python
# Command Interface
class Command:
    def execute(self):
        pass

    def undo(self):
        pass

# Concrete Commands
class WriteCommand(Command):
    def __init__(self, editor, words):
        self.editor = editor
        self.words = words
        self.previous_text = editor.text

    def execute(self):
        self.editor.text += self.words

    def undo(self):
        self.editor.text = self.previous_text

class DeleteCommand(Command):
    def __init__(self, editor):
        self.editor = editor
        self.previous_text = editor.text

    def execute(self):
        self.editor.text = self.editor.text[:-1]

    def undo(self):
        self.editor.text = self.previous_text

# Receiver
class TextEditor:
    def __init__(self):
        self.text = ""
        self.clipboard = ""

# Invoker
class CommandManager:
    def __init__(self):
        self.history = []

    def execute_command(self, command):
        command.execute()
        self.history.append(command)

    def undo(self):
        if self.history:
            command = self.history.pop()
            command.undo()

# Usage
editor = TextEditor()
manager = CommandManager()

write_cmd = WriteCommand(editor, "Hello, ")
manager.execute_command(write_cmd)

delete_cmd = DeleteCommand(editor)
manager.execute_command(delete_cmd)

print(editor.text)  # Output: Hello

manager.undo()
print(editor.text)  # Output: Hello,
```

**Improvements**: 
- **[[Decoupling in Software Development|Decoupling]]**: The command manager is decoupled from the editor's actions. It only knows about the command interface.
- **Undo/Redo functionality**: You can easily implement undo/redo functionality by maintaining a history of commands.
- **Macro functionality**: You can easily create macros by executing a series of commands.

In both examples, the Command pattern encapsulates each request as an object, thereby allowing for parameterization of clients with queues, requests, and operations. It provides the flexibility to queue operations, schedule their execution, and execute them remotely. This pattern is particularly useful in scenarios where you need to support undoable actions, logging changes, transactions, or macro recording. The resulting architecture is more maintainable, scalable, and adaptable to changes.
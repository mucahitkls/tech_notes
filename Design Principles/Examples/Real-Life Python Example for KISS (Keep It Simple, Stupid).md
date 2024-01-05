
Let's consider a real-world application of a task management system for a small team. The system allows team members to add, edit, delete, and view tasks. Initially, the system is designed without keeping the KISS principle in mind, leading to unnecessarily complex and hard-to-maintain code.

## Before Applying KISS

### Initial System:

- `TaskManager`: A class that handles all task operations with complex logic and data structures.

#### task_manager.py (Before)

```python
class TaskManager:
    def __init__(self):
        # Using a complex data structure to store tasks
        self.tasks = {"open": [], "in_progress": [], "completed": []}

    def add_task(self, task, status="open"):
        if status not in self.tasks:
            print(f"Unknown status {status}. Cannot add task.")
            return
        self.tasks[status].append(task)
        print(f"Added task: {task} with status {status}")

    def change_task_status(self, task, new_status):
        if new_status not in self.tasks:
            print(f"Unknown status {new_status}. Cannot change task status.")
            return
        for status in self.tasks:
            if task in self.tasks[status]:
                self.tasks[status].remove(task)
                self.tasks[new_status].append(task)
                print(f"Moved task: {task} to {new_status}")
                return
        print(f"Task {task} not found.")

    def show_tasks(self, status=None):
        if status and status in self.tasks:
            print(f"Tasks with status {status}: {self.tasks[status]}")
            return
        for status, tasks in self.tasks.items():
            print(f"Tasks with status {status}: {tasks}")

# Usage
manager = TaskManager()
manager.add_task("Implement KISS")
manager.add_task("Implement OCP", "in_progress")
manager.change_task_status("Implement KISS", "in_progress")
manager.show_tasks()
```

In this non-KISS-compliant design, the `TaskManager` class uses a complex data structure to manage tasks and has methods that are more complicated than necessary.

## After Applying KISS

### Refactored System:

- Simplify the data structure and methods in the `TaskManager` class.

#### task_manager.py (After)

```python
class Task:
    def __init__(self, description, status="open"):
        self.description = description
        self.status = status

class TaskManager:
    def __init__(self):
        self.tasks = []

    def add_task(self, task):
        self.tasks.append(task)
        print(f"Added task: {task.description} with status {task.status}")

    def change_task_status(self, task_description, new_status):
        for task in self.tasks:
            if task.description == task_description:
                task.status = new_status
                print(f"Task '{task_description}' status changed to {new_status}")
                return
        print(f"Task '{task_description}' not found.")

    def show_tasks(self, status=None):
        for task in self.tasks:
            if status is None or task.status == status:
                print(f"Task: {task.description}, Status: {task.status}")

# Usage
manager = TaskManager()
manager.add_task(Task("Implement KISS"))
manager.add_task(Task("Implement OCP", "in_progress"))
manager.change_task_status("Implement KISS", "in_progress")
manager.show_tasks()
```

## Conclusion

In the refactored example:

- The `TaskManager` class now uses a simple list to store tasks and straightforward methods to interact with them.
- A `Task` class is introduced to represent each task, making it easier to manage individual task properties.
- The code is significantly simplified, making it easier to read, understand, and maintain.

This KISS-compliant design reduces complexity and increases the maintainability of the code. It's easier to manage and update the task management system as new requirements emerge. The simpler design also makes it more straightforward for other developers to understand and contribute to the codebase. This example emphasizes the benefits of adhering to the KISS principle in a real-world application, leading to a cleaner, more efficient, and maintainable codebase.
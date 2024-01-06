# JavaScript Example of Encapsulation

## Scenario: User Account Management System

In an application that manages user accounts, we want to ensure that the user's sensitive information is protected and only modified in controlled ways.

### Without Encapsulation:

Initially, we might have a simple User class where all properties are publicly accessible, leading to potential misuse.

```javascript
class User {
    constructor(username, age, password) {
        this.username = username;
        this.age = age;
        this.password = password;  // Sensitive information
    }
}

// Usage
let user = new User("JohnDoe", 30, "superSecret");

// Directly accessing and modifying properties
console.log(user.password);  // superSecret
user.password = "newPassword";
```

**Issues**: Direct access to sensitive information like passwords is a security risk. There's no control over how and when data is modified.

### After Introducing Encapsulation:

We introduce a more secure User class where sensitive details are hidden and can only be accessed or modified through methods that implement checks and logic.

```javascript
class User {
    #password;  // Private field

    constructor(username, age, password) {
        this.username = username;
        this.age = age;
        this.#password = password;
    }

    // Getter for password (could implement logic for who can access it)
    getPassword() {
        return this.#password;
    }

    // Setter for password (could implement password strength checks)
    setPassword(newPassword) {
        if (newPassword.length > 8) {
            this.#password = newPassword;
        } else {
            console.log("Password too short!");
        }
    }
}

// Usage
let user = new User("JohnDoe", 30, "superSecret");

// Attempting to access private property
console.log(user.#password);  // SyntaxError: Private field '#password' must be declared in an enclosing class

// Controlled access and modification
console.log(user.getPassword());  // superSecret
user.setPassword("short");         // Password too short!
user.setPassword("newLongPassword");
console.log(user.getPassword());  // newLongPassword
```

**Improvements**: 
- **Security**: Sensitive information like the password is now hidden from outside access.
- **Controlled Access**: Changes to the password now go through a setter method, which can implement necessary checks.
- **Maintainability**: The internal representation of the password can change without affecting other parts of the system.

# Python Example of Encapsulation

## Scenario: Bank Account Management

In a banking system, we want to ensure that customers' balance information is secure and that updates to the balance follow business rules.

### Without Encapsulation:

Initially, account details might be directly accessible and modifiable, leading to potential misuse.

```python
class BankAccount:
    def __init__(self, account_number, balance):
        self.account_number = account_number
        self.balance = balance  # Sensitive information

# Usage
account = BankAccount(123456, 1000)

# Directly accessing and modifying properties
print(account.balance)  # 1000
account.balance += 500  # Increasing balance without any checks
```

**Issues**: The balance can be modified directly, without any validation or business rule enforcement.

### After Introducing Encapsulation:

We introduce a secure BankAccount class where the balance is hidden and can only be accessed or modified through methods that implement checks and business logic.

```python
class BankAccount:
    def __init__(self, account_number, balance):
        self.account_number = account_number
        self.__balance = balance  # Private attribute

    # Getter for balance (could implement access control)
    def get_balance(self):
        return self.__balance

    # Method to deposit money (could implement validation)
    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount
        else:
            print("Invalid deposit amount!")

    # Method to withdraw money (could implement checks and business rules)
    def withdraw(self, amount):
        if 0 < amount <= self.__balance:
            self.__balance -= amount
        else:
            print("Invalid or insufficient funds!")

# Usage
account = BankAccount(123456, 1000)

# Attempting to access private attribute
print(account.__balance)  # AttributeError: 'BankAccount' object has no attribute '__balance'

# Controlled access and modification
print(account.get_balance())  # 1000
account.deposit(500)
print(account.get_balance())  # 1500
account.withdraw(2000)        # Invalid or insufficient funds!
```

**Improvements**: 
- **Security**: The balance is now protected from direct outside access.
- **Controlled Modification**: Deposits and withdrawals now go through methods that can enforce business rules and validation.
- **Flexibility**: The way balance is stored and managed can change internally without affecting other parts of the system.

In both JavaScript and Python examples, encapsulation has transformed the design, making it more secure, maintainable, and robust. This protects sensitive information and ensures that data is only modified in controlled, acceptable ways. The use of encapsulation also introduces a clear structure and separation of concerns, making the systems easier to understand and manage.

Consider a real-world application of a hotel management system. This system manages various aspects such as room booking, customer details, and billing. Initially, the system isn't designed with encapsulation in mind, leading to less secure and maintainable code. We'll refactor it to use encapsulation, highlighting the benefits of this principle.

## Before Applying Encapsulation

### Initial System:

- `Customer` and `Room` classes with public attributes.
- `BookingManager` handling booking operations directly interacting with other class's data.

#### hotel_management.py (Before)

```python
class Customer:
    def __init__(self, name, age):
        self.name = name
        self.age = age
        self.balance = 0

class Room:
    def __init__(self, number, rate):
        self.number = number
        self.rate = rate
        self.is_occupied = False

class BookingManager:
    def book_room(self, customer, room):
        if room.is_occupied:
            print(f"Room {room.number} is already occupied.")
            return False
        room.is_occupied = True
        customer.balance -= room.rate
        print(f"Room {room.number} is booked by {customer.name}.")
        return True

# Usage
customer = Customer("John Doe", 30)
room = Room(101, 100)
manager = BookingManager()
manager.book_room(customer, room)
```

In this non-encapsulated design, `BookingManager` directly interacts with `Customer` and `Room` attributes, which are publicly accessible.

## After Applying Encapsulation

### Refactored System:

- Add private attributes and public methods to `Customer` and `Room` classes.
- `BookingManager` interacts with `Customer` and `Room` through methods ensuring data integrity.

#### hotel_management.py (After)

```python
class Customer:
    def __init__(self, name, age):
        self.__name = name
        self.__age = age
        self.__balance = 0

    def make_payment(self, amount):
        if amount > 0:
            self.__balance -= amount
            return True
        return False

    def get_name(self):
        return self.__name

class Room:
    def __init__(self, number, rate):
        self.__number = number
        self.__rate = rate
        self.__is_occupied = False

    def book(self):
        if self.__is_occupied:
            return False
        self.__is_occupied = True
        return True

    def get_rate(self):
        return self.__rate

    def get_number(self):
        return self.__number

class BookingManager:
    def book_room(self, customer, room):
        if not room.book():
            print(f"Room {room.get_number()} is already occupied.")
            return False
        if customer.make_payment(room.get_rate()):
            print(f"Room {room.get_number()} is booked by {customer.get_name()}.")
            return True
        print("Payment failed.")
        return False

# Usage
customer = Customer("John Doe", 30)
room = Room(101, 100)
manager = BookingManager()
manager.book_room(customer, room)
```

## Conclusion

In the refactored example:

- `Customer` and `Room` classes now have private attributes with public methods to interact with those attributes, adhering to encapsulation.
- `BookingManager` interacts with `Customer` and `Room` through these public methods, ensuring that the internal state of these objects is maintained and valid.
- The system is more secure and maintainable. It's harder to put `Customer` and `Room` into an invalid state from outside the class.

This encapsulated design provides a more secure, maintainable, and flexible structure. It protects the integrity of data within `Customer` and `Room` and ensures that all interactions with these objects go through a controlled and predictable interface. The example emphasizes the benefits of adhering to the Encapsulation principle in a real-world application, leading to a cleaner, more robust, and maintainable codebase.
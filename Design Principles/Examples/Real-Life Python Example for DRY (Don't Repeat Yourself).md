
Let's consider a real-world application of an e-commerce platform that handles various types of notifications: order confirmation, shipping updates, and promotional messages. Initially, the system isn't designed with the DRY principle in mind, leading to repetitive and less maintainable code.

## Before Applying DRY

### Initial System:

- Different methods for each type of notification with repeated logic for sending messages.

#### notification_system.py (Before)

```python
class NotificationSystem:
    def __init__(self):
        self.server = "smtp.emailserver.com"

    def send_order_confirmation(self, user_email, order_id):
        message = f"Thank you for your order! Your order ID is {order_id}."
        print(f"Connecting to {self.server}")
        print(f"Sending to {user_email}: {message}")

    def send_shipping_update(self, user_email, order_id, status):
        message = f"Your order {order_id} is now {status}."
        print(f"Connecting to {self.server}")
        print(f"Sending to {user_email}: {message}")

    def send_promotion(self, user_email, promotion_info):
        message = f"Check out our new promotion: {promotion_info}!"
        print(f"Connecting to {self.server}")
        print(f"Sending to {user_email}: {message}")

# Usage
notifier = NotificationSystem()
notifier.send_order_confirmation("user@example.com", 1234)
notifier.send_shipping_update("user@example.com", 1234, "shipped")
notifier.send_promotion("user@example.com", "50% off!")
```

In this non-DRY-compliant design, the logic for connecting to the server and sending the message is repeated in each method.

## After Applying DRY

### Refactored System:

- A single method to handle the message sending process.
- Specific methods for each notification type now only construct their respective messages.

#### notification_system.py (After)

```python
class NotificationSystem:
    def __init__(self):
        self.server = "smtp.emailserver.com"

    def send_message(self, user_email, message):
        print(f"Connecting to {self.server}")
        print(f"Sending to {user_email}: {message}")

    def order_confirmation(self, order_id):
        return f"Thank you for your order! Your order ID is {order_id}."

    def shipping_update(self, order_id, status):
        return f"Your order {order_id} is now {status}."

    def promotion(self, promotion_info):
        return f"Check out our new promotion: {promotion_info}!"

# Usage
notifier = NotificationSystem()
message = notifier.order_confirmation(1234)
notifier.send_message("user@example.com", message)

message = notifier.shipping_update(1234, "shipped")
notifier.send_message("user@example.com", message)

message = notifier.promotion("50% off!")
notifier.send_message("user@example.com", message)
```

## Conclusion

In the refactored example:

- The `send_message` method centralizes the logic for connecting to the server and sending messages, adhering to DRY.
- The `order_confirmation`, `shipping_update`, and `promotion` methods are responsible for constructing their respective messages.
- The system is now more maintainable and scalable. Adding a new type of notification doesn't require duplicating the message-sending logic.

This DRY-compliant design reduces repetition and increases the maintainability of the code. It's easier to manage and update the notification system as new requirements emerge. The centralized `send_message` method also makes it simpler to modify the message-sending process (e.g., changing the server or adding logging) without touching the rest of the code. This example emphasizes the benefits of adhering to the DRY principle in a real-world application, leading to a cleaner, more efficient, and maintainable codebase.
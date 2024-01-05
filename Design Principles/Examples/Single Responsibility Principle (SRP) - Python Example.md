## Introduction to the Example

We'll use a fictional project that represents a simple content management system (CMS) for blog posts. This CMS allows for adding, editing, and deleting posts, and also includes basic analytics functionality. Initially, the project will be designed without considering SRP, leading to classes with multiple responsibilities. Then, we'll refactor the code to adhere to SRP, resulting in a more maintainable and clear structure.

## Before Applying SRP

### Initial Project Structure:

- `BlogManager`: Manages blog posts and analytics.
- `Post`: Represents a blog post.

### blog_manager.py (Before)

```python
class Post:
    def __init__(self, title, content):
        self.title = title
        self.content = content

class BlogManager:
    def __init__(self):
        self.posts = []

    def add_post(self, title, content):
        new_post = Post(title, content)
        self.posts.append(new_post)
        print(f"Added: {title}")

    def edit_post(self, title, new_title, new_content):
        for post in self.posts:
            if post.title == title:
                post.title = new_title
                post.content = new_content
                print(f"Edited: {new_title}")
                return
        print("Post not found.")

    def delete_post(self, title):
        for i, post in enumerate(self.posts):
            if post.title == title:
                del self.posts[i]
                print(f"Deleted: {title}")
                return
        print("Post not found.")

    def display_all_posts(self):
        print("All Posts:")
        for post in self.posts:
            print(f"{post.title}: {post.content}")

    def total_word_count(self):
        return sum(len(post.content.split()) for post in self.posts)

    def posts_containing_word(self, word):
        count = sum(word in post.content for post in self.posts)
        print(f"Number of posts containing '{word}': {count}")

# Usage
manager = BlogManager()
manager.add_post("SRP", "Single Responsibility Principle in detail...")
manager.add_post("OOP", "Object-oriented programming basics...")
manager.display_all_posts()
print(f"Total word count: {manager.total_word_count()}")
manager.posts_containing_word("basics")
```

In this example, the `BlogManager` class is handling multiple responsibilities - managing posts (add, edit, delete, display) and also performing analytics (word count, search for word occurrence). This violates the SRP.

## After Applying SRP

### Refactored Project Structure:

- `Post`: Represents a blog post (Unchanged).
- `PostManager`: Manages blog posts (add, edit, delete, display).
- `AnalyticsManager`: Handles analytics-related tasks (word count, search).

### post_manager.py (After)

```python
class PostManager:
    def __init__(self):
        self.posts = []

    def add_post(self, title, content):
        new_post = Post(title, content)
        self.posts.append(new_post)
        print(f"Added: {title}")

    def edit_post(self, title, new_title, new_content):
        for post in self.posts:
            if post.title == title:
                post.title = new_title
                post.content = new_content
                print(f"Edited: {new_title}")
                return
        print("Post not found.")

    def delete_post(self, title):
        for i, post in enumerate(self.posts):
            if post.title == title:
                del self.posts[i]
                print(f"Deleted: {title}")
                return
        print("Post not found.")

    def display_all_posts(self):
        print("All Posts:")
        for post in self.posts:
            print(f"{post.title}: {post.content}")

    def get_all_posts(self):
        return self.posts
```

### analytics_manager.py (After)

```python
class AnalyticsManager:
    @staticmethod
    def total_word_count(posts):
        return sum(len(post.content.split()) for post in posts)

    @staticmethod
    def posts_containing_word(posts, word):
        count = sum(word in post.content for post in posts)
        print(f"Number of posts containing '{word}': {count}")
```

### Usage (After)

```python
post_manager = PostManager()
post_manager.add_post("SRP", "Single Responsibility Principle in detail...")
post_manager.add_post("OOP", "Object-oriented programming basics...")
post_manager.display_all_posts()

analytics_manager = AnalyticsManager()
print(f"Total word count: {analytics_manager.total_word_count(post_manager.get_all_posts())}")
analytics_manager.posts_containing_word(post_manager.get_all_posts(), "basics")
```

## Conclusion

In the refactored example:

- The `PostManager` class is now responsible only for managing posts. It's focused and adheres to SRP.
- The `AnalyticsManager` class is introduced to handle all analytics-related tasks. It's separate from post management and can be modified independently.
- Each class has a single reason to change, making the system more maintainable and flexible.

By applying the Single Responsibility Principle, we've improved the design of our CMS system. It's now more organized, with clear boundaries between different functionalities. This makes it easier to understand, maintain, and extend. As the system grows, adhering to SRP will continue to provide benefits in terms of simplicity and manageability.
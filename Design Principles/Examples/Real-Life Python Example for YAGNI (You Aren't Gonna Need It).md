
Imagine a real-world application like a simple content management system (CMS) for a blog. This system allows users to create, edit, delete, and view blog posts. Initially, the system is designed without keeping the YAGNI principle in mind, leading to unnecessary features and complexity.

## Before Applying YAGNI

### Initial System:

- `BlogPostManager`: Handles blog post operations with additional speculative features.
- `AdvancedSearch`: An elaborate, unused search feature.
- `Analytics`: An advanced, yet unused analytics system.

#### blog_system.py (Before)

```python
import datetime

class BlogPost:
    def __init__(self, title, content, author):
        self.title = title
        self.content = content
        self.author = author
        self.created_at = datetime.datetime.now()
        self.comments = []

    def add_comment(self, comment):
        self.comments.append(comment)

class AdvancedSearch:
    def __init__(self):
        pass  # Complex unused search logic

    def perform_search(self, query):
        print(f"Performing complex search for {query}")

class Analytics:
    def __init__(self, posts):
        self.posts = posts

    def calculate_read_time(self):
        pass  # Complex unused analytics logic

class BlogPostManager:
    def __init__(self):
        self.posts = []
        self.search = AdvancedSearch()
        self.analytics = Analytics(self.posts)

    def create_post(self, title, content, author):
        new_post = BlogPost(title, content, author)
        self.posts.append(new_post)

    def edit_post(self, post_id, new_title, new_content):
        # Edit post logic

    def delete_post(self, post_id):
        # Delete post logic

    def show_all_posts(self):
        for post in self.posts:
            print(f"Title: {post.title}, Author: {post.author}")

# Usage
manager = BlogPostManager()
manager.create_post("YAGNI in Practice", "Applying YAGNI is crucial for clean code.", "Dev")
manager.show_all_posts()
```

In this non-YAGNI-compliant design, `BlogPostManager` integrates with an `AdvancedSearch` and `Analytics` feature that isn't required at the moment. These speculative features add unnecessary complexity.

## After Applying YAGNI

### Refactored System:

- Simplify the `BlogPostManager` to handle only the current requirements.
- Remove the speculative `AdvancedSearch` and `Analytics` features.

#### blog_system.py (After)

```python
import datetime

class BlogPost:
    def __init__(self, title, content, author):
        self.title = title
        self.content = content
        self.author = author
        self.created_at = datetime.datetime.now()
        self.comments = []

    def add_comment(self, comment):
        self.comments.append(comment)

class BlogPostManager:
    def __init__(self):
        self.posts = []

    def create_post(self, title, content, author):
        new_post = BlogPost(title, content, author)
        self.posts.append(new_post)

    def edit_post(self, post_id, new_title, new_content):
        # Edit post logic (simplified for brevity)

    def delete_post(self, post_id):
        # Delete post logic (simplified for brevity)

    def show_all_posts(self):
        for post in self.posts:
            print(f"Title: {post.title}, Author: {post.author}")

# Usage
manager = BlogPostManager()
manager.create_post("YAGNI in Practice", "Applying YAGNI is crucial for clean code.", "Dev")
manager.show_all_posts()
```

## Conclusion

In the refactored example:

- The `BlogPostManager` class is now focused solely on managing blog posts. It's simpler and more aligned with the current requirements.
- Unused `AdvancedSearch` and `Analytics` classes are removed, adhering to YAGNI.
- The system is more maintainable and easier to understand. New features can be added as needed without wading through unnecessary code.

This YAGNI-compliant design reduces unnecessary complexity and increases the maintainability of the code. It focuses development efforts on what's truly needed, making the system easier to extend and adapt as new requirements emerge. The example emphasizes the benefits of adhering to the YAGNI principle in a real-world application, leading to a cleaner, more efficient, and maintainable codebase.

Consider a real-world application of a multimedia content system that manages various types of media, such as text articles, audio podcasts, and video episodes. Initially, the system is designed using inheritance, which leads to a rigid and less flexible structure. We'll refactor it to use composition, highlighting the benefits of this principle.

## Before Applying Composition Over Inheritance

### Initial System:

- Base class `MediaContent` with derived classes `Article`, `Podcast`, and `Video`.

#### media_content.py (Before)

```python
class MediaContent:
    def __init__(self, title, creator):
        self.title = title
        self.creator = creator

class Article(MediaContent):
    def __init__(self, title, creator, text):
        super().__init__(title, creator)
        self.text = text

    def display(self):
        print(f"Article: {self.title} by {self.creator}")
        print(self.text)

class Podcast(MediaContent):
    def __init__(self, title, creator, audio_file):
        super().__init__(title, creator)
        self.audio_file = audio_file

    def play(self):
        print(f"Playing podcast: {self.title} by {self.creator}")

class Video(MediaContent):
    def __init__(self, title, creator, video_file):
        super().__init__(title, creator)
        self.video_file = video_file

    def play(self):
        print(f"Playing video: {self.title} by {self.creator}")

# Usage
article = Article("Inheritance vs. Composition", "Alice", "Content of the article...")
article.display()

podcast = Podcast("Understanding Composition", "Bob", "audio.mp3")
podcast.play()

video = Video("Benefits of Composition", "Charlie", "video.mp4")
video.play()
```

In this non-composition-based design, each media type extends `MediaContent`, leading to a hierarchy that could become complex and inflexible as more media types and features are added.

## After Applying Composition Over Inheritance

### Refactored System:

- A single class `MediaContent` and separate classes for behaviors like `DisplayableText`, `PlayableAudio`, and `PlayableVideo`.

#### media_content.py (After)

```python
class MediaContent:
    def __init__(self, title, creator, content_behavior):
        self.title = title
        self.creator = creator
        self.content_behavior = content_behavior

    def publish(self):
        print(f"Publishing {self.title} by {self.creator}")
        self.content_behavior.execute()

class DisplayableText:
    def __init__(self, text):
        self.text = text

    def execute(self):
        print(self.text)

class PlayableAudio:
    def __init__(self, audio_file):
        self.audio_file = audio_file

    def execute(self):
        print(f"Playing audio file {self.audio_file}")

class PlayableVideo:
    def __init__(self, video_file):
        self.video_file = video_file

    def execute(self):
        print(f"Playing video file {self.video_file}")

# Usage
article_content = DisplayableText("Content of the article...")
article = MediaContent("Inheritance vs. Composition", "Alice", article_content)
article.publish()

podcast_content = PlayableAudio("audio.mp3")
podcast = MediaContent("Understanding Composition", "Bob", podcast_content)
podcast.publish()

video_content = PlayableVideo("video.mp4")
video = MediaContent("Benefits of Composition", "Charlie", video_content)
video.publish()
```

## Conclusion

In the refactored example:

- `MediaContent` is now a single class that doesn't inherit from other classes. Instead, it composes its behavior through `content_behavior`.
- `DisplayableText`, `PlayableAudio`, and `PlayableVideo` are behavior classes that can be attached to `MediaContent`.
- The system is now more flexible and maintainable. Adding a new media type or content behavior doesn't require changing the existing class hierarchy.

This composition-based design provides a more flexible and maintainable structure. It's easier to extend with new content types and behaviors without altering existing code. The example emphasizes the benefits of adhering to the Composition Over Inheritance principle in a real-world application, leading to a cleaner, more adaptable, and maintainable codebase.
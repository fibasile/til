---
published: false
layout: article
category: frontend
tags:
  - flutter
  - state
  - redux
  - bloc
---
## Managing state in Flutter

State management in Flutter feels quite a debated topic. The [official documentation](https://flutter.io/docs/development/data-and-backend/state-mgmt) itself doesn't give any clear recommendation other that of reading a ton of articles!

I'll try to make a quick summary of what I learned reading (a few) of them.

The main options for state management are described in the next sections, followed a comparison based on various use cases.

### setState

The built-in mechanism for state management at widget level. In short 
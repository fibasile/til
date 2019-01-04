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

### Flutter setState

The built-in mechanism for state management at widget level. 

This is based on [StatefulWidget](https://docs.flutter.io/flutter/widgets/StatefulWidget-class.html). 

Reactivity comes from delegating building the widget to a State object, that will also take care of rebuilding it every time any attribute is changed (using the setState method, more below). 

In short, the Widget returns a state object of type `State<Widget>` where state is initialized in `initState` and updated with `setState`, widget is built in the `build` method. The state itself is represented by attributes of the `State` subclass.

I see this option as an easy solution for managing UI-related state, without resorting to more complex cases.

### Streams

### BLoC

The Business Logic Components pattern was introduced by the Flutter devs, if I'm not wrong, during GoogleIO 2018. BLoC is clearly inspired by streams.

<iframe width="560" height="315" src="https://www.youtube.com/embed/fahC3ky_zW0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>



References: 

- [Flutter BLoC](https://pub.dartlang.org/packages/flutter_bloc)
- [BLoC dart package](https://pub.dartlang.org/packages/bloc)


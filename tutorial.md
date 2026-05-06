# Flutter Basics — From Empty File to Interactive App

A step-by-step guide for students. We'll start from a blank Dart file, build up to widgets and buttons, then move from **Stateless** to **Stateful**, and finish with customization tips.

---

## Step 0 — The Default Template (What Flutter Gives You)

When you run `flutter create my_app`, Flutter generates a project with a default counter app already inside `lib/main.dart`. It looks something like this:

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.green),
      ),
      home: const MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key, required this.title});
  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text('You have pushed the button this many times:'),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.headlineMedium,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

This already works! Run it and you'll see a counter app. But to **really learn** Flutter, we won't just edit this file — we'll erase it and rebuild it ourselves, line by line.

> **Action for students:** Delete everything inside `lib/main.dart`. We're starting from a blank file.

---

## Step 1 — The True Starting Point

With `lib/main.dart` empty, type this:

```dart
void main() {
  // This is where our app will start
}
```

A `main()` function is the entry point of every Dart program — just like in C, Java, or Go. Right now, this app does nothing. Let's give it something to display.

---

## Step 2 — Import Flutter

Before we can use any Flutter widgets, we need to import the Material library. Material gives us pre-built widgets that follow Google's Material Design.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}
```

**What changed?**
- `import 'package:flutter/material.dart';` → gives us access to widgets like `Text`, `Scaffold`, `AppBar`, etc.
- `runApp(...)` → tells Flutter to start the app and display the widget we pass in.
- `const MyApp()` → the root widget of our app. We haven't built it yet, so the next step is to create it.

---

## Step 3 — Your First Stateless Widget

A **StatelessWidget** is a widget that doesn't change. It just displays something. Think of it like a poster on a wall — it looks the same every time you see it.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: const Text('Hello Flutter')),
        body: const Center(
          child: Text('Hello, students!'),
        ),
      ),
    );
  }
}
```

**Breakdown:**
- `MaterialApp` → wraps your app, gives it Material styling.
- `Scaffold` → the basic page layout (has space for app bar, body, floating button, etc.).
- `AppBar` → the bar at the top of the screen.
- `Center` → centers its child on the screen.
- `Text(...)` → displays text.

Run the app. You should see "Hello, students!" in the middle of the screen.

---

## Step 4 — Add More Widgets (Column & Row)

To stack widgets vertically, use `Column`. For horizontal, use `Row`.

```dart
body: Center(
  child: Column(
    mainAxisAlignment: MainAxisAlignment.center,
    children: const [
      Text('Welcome!'),
      SizedBox(height: 20), // spacing
      Text('This is Flutter.'),
      SizedBox(height: 20),
      Icon(Icons.flutter_dash, size: 80, color: Colors.blue),
    ],
  ),
),
```

**Tips:**
- `SizedBox(height: 20)` → adds vertical spacing.
- `Icon(...)` → built-in icons. Try `Icons.favorite`, `Icons.star`, etc.
- `mainAxisAlignment` → controls how children are aligned along the main axis.

---

## Step 5 — Add a Button

Let's add a button. We'll use `ElevatedButton` for now — the most common Flutter button.

```dart
body: Center(
  child: Column(
    mainAxisAlignment: MainAxisAlignment.center,
    children: [
      const Text('Press the button below:'),
      const SizedBox(height: 20),
      ElevatedButton(
        onPressed: () {
          print('Button was pressed!');
        },
        child: const Text('Click Me'),
      ),
    ],
  ),
),
```

**What's happening?**
- `onPressed: () { ... }` → this function runs every time the button is pressed.
- `print(...)` → prints to the debug console (visible in your IDE).

Other button types to try later: `TextButton`, `OutlinedButton`, `IconButton`.

---

## Step 6 — From Stateless to Stateful

Right now our button prints a message but **nothing on the screen changes**. To make the UI react to user actions, we need a **StatefulWidget**.

A StatefulWidget can hold values that change (like a counter, a toggle, or input text).

Here's the classic counter example:

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(home: CounterPage());
  }
}

class CounterPage extends StatefulWidget {
  const CounterPage({super.key});

  @override
  State<CounterPage> createState() => _CounterPageState();
}

class _CounterPageState extends State<CounterPage> {
  int counter = 0; // this value can change

  void increment() {
    setState(() {
      counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Counter App')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text('You pressed the button this many times:'),
            const SizedBox(height: 10),
            Text(
              '$counter',
              style: const TextStyle(fontSize: 40, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: increment,
              child: const Text('Add +1'),
            ),
          ],
        ),
      ),
    );
  }
}
```

**Key concepts:**
- A `StatefulWidget` has two classes: the widget itself and a `State` class.
- `setState(() { ... })` → tells Flutter "something changed, please rebuild the screen."
- Without `setState`, the variable updates but the UI won't refresh.

> **Rule of thumb:** If your widget never changes → Stateless. If it has data that updates over time → Stateful.

---

## Step 7 — Customization

Now let's make things look nicer. Most widgets accept styling parameters.

### Customizing Text

```dart
Text(
  'Stylish Text',
  style: TextStyle(
    fontSize: 28,
    fontWeight: FontWeight.bold,
    color: Colors.deepPurple,
    letterSpacing: 2,
    fontStyle: FontStyle.italic,
  ),
)
```

### Customizing Buttons

```dart
ElevatedButton(
  onPressed: () {},
  style: ElevatedButton.styleFrom(
    backgroundColor: Colors.teal,       // button color
    foregroundColor: Colors.white,      // text color
    padding: const EdgeInsets.symmetric(horizontal: 30, vertical: 15),
    shape: RoundedRectangleBorder(
      borderRadius: BorderRadius.circular(12),
    ),
  ),
  child: const Text('Pretty Button', style: TextStyle(fontSize: 18)),
)
```

### Customizing Containers (the styling powerhouse)

`Container` is your best friend for backgrounds, borders, padding, and shadows.

```dart
Container(
  padding: const EdgeInsets.all(20),
  margin: const EdgeInsets.all(10),
  decoration: BoxDecoration(
    color: Colors.amber.shade100,
    borderRadius: BorderRadius.circular(16),
    border: Border.all(color: Colors.orange, width: 2),
    boxShadow: [
      BoxShadow(
        color: Colors.black.withOpacity(0.2),
        blurRadius: 8,
        offset: const Offset(0, 4),
      ),
    ],
  ),
  child: const Text('Inside a styled container'),
)
```

### Customizing the AppBar

```dart
AppBar(
  title: const Text('My App'),
  backgroundColor: Colors.indigo,
  foregroundColor: Colors.white,
  elevation: 4,
  centerTitle: true,
)
```

### App-wide theme

Instead of styling each widget, set a theme once in `MaterialApp`:

```dart
MaterialApp(
  theme: ThemeData(
    primarySwatch: Colors.purple,
    scaffoldBackgroundColor: Colors.grey.shade100,
    textTheme: const TextTheme(
      bodyMedium: TextStyle(fontSize: 16),
    ),
  ),
  home: const CounterPage(),
)
```

---

## Step 8 — Mini Challenge

Try to build this on your own:

1. A page with the title "My Profile".
2. A circular avatar (use `CircleAvatar`).
3. Your name as bold text below it.
4. Two buttons: "Like" and "Reset".
5. A counter that increments when "Like" is pressed and goes back to zero when "Reset" is pressed.

**Hints:**
- Use a `StatefulWidget`.
- `CircleAvatar(radius: 50, backgroundColor: Colors.blue, child: Icon(Icons.person, size: 50, color: Colors.white))`
- Wrap the two buttons inside a `Row` with `mainAxisAlignment: MainAxisAlignment.center`.

---

## Quick Reference Cheat Sheet

| Widget | Purpose |
|---|---|
| `MaterialApp` | Root of the app |
| `Scaffold` | Page structure |
| `AppBar` | Top bar |
| `Text` | Display text |
| `Icon` | Display an icon |
| `Container` | Styled box |
| `Column` / `Row` | Vertical / horizontal layout |
| `Center` | Centers a child |
| `SizedBox` | Spacing or fixed size |
| `ElevatedButton` | Filled button |
| `TextButton` | Flat button |
| `IconButton` | Tappable icon |
| `CircleAvatar` | Circular profile image |

---

## Summary

1. Every app starts with `void main()` and `runApp()`.
2. **StatelessWidget** = static UI. **StatefulWidget** = changing UI.
3. Use `setState()` to update the screen.
4. Use `Container`, `TextStyle`, `BoxDecoration`, and `ThemeData` to make things pretty.
5. Build small, then combine.

Happy coding! 🚀
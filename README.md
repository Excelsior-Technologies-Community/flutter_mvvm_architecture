# MVVM Architecture
A concise guide for implementing the Model-View-ViewModel (MVVM) architecture pattern in Flutter.

---
## 1️⃣ What is MVVM?
MVVM = Model – View – ViewModel

MVVM is an architecture pattern that separates UI from logic and data.

### Why MVVM?
* Clean code
* Easy to scale
* Easy to test
* UI stays simple
* Business logic stays reusable

---
## 2️⃣ MVVM Layers Explained (Very Clearly)
## 🧩 Model
#### What it is
* Data structure only
* Represents API / database data
#### What it should NOT do
* No UI
* No state management
* No API calls

## Example
```
class User {
  final int id;
  final String name;

  User({required this.id, required this.name});
}
```
## 🎨 View
### What it is
* Flutter UI (Widgets)
* Displays data
* Handles user interaction (button taps)
### What it should NOT do
* No business logic
* No API calls
* No data processing
#### View only talks to → ViewModel

## 🧠 ViewModel
### What it is
* Holds app state
* Contains business logic
* Communicates with models/services
* Notifies UI when data changes
### Role
 ViewModel is the **brain** of the screen.
 
 ---
## 3️⃣ How MVVM Works (Flow)
```
User taps button (View)
        ↓
View calls ViewModel method
        ↓
ViewModel updates data/state
        ↓
ViewModel notifies listeners
        ↓
View rebuilds automatically
```
---
## 4️⃣ State Management in MVVM
### Best simple choice in Flutter:
### ✅ Provider + ChangeNotifier
#### Why?
* Official recommendation
* Easy to understand
* Perfect for MVVM
* No boilerplate
  
  ---
## 5️⃣ Clean Folder Structure (Recommended)
```
  
lib/
├── main.dart
│
├── core/                      # App-wide utilities
│   ├── services/              # API, location, storage
│   │   └── api_service.dart
│   └── constants/
│
├── data/                      # MODELS
│   └── models/
│       └── user_model.dart
│
├── view/                      # UI Layer
│   ├── screens/
│   │   └── home_view.dart
│   └── widgets/
│
├── view_model/                # STATE + LOGIC
│   └── user_view_model.dart
│
└── routes/
    └── app_routes.dart


```
---

## Folder Responsibility Table

| Folder     | Role                      |
| ---------- | ------------------------- |
| core       | Shared services & helpers |
| data       | Models only               |
| view       | UI only                   |
| view_model | State & business logic    |
| routes     | Navigation                |


---
## 6️⃣ Simple Working Example
### Model
##### 📁 models/user_model.dart
```
class User {
  final String name;
  User(this.name);
}
```
### ViewModel
##### 📁 view_models/user_view_model.dart
```
import 'package:flutter/material.dart';
import '../models/user_model.dart';

class UserViewModel extends ChangeNotifier {
  User? _user;
  bool _loading = false;

  User? get user => _user;
  bool get loading => _loading;

  void loadUser() async {
    _loading = true;
    notifyListeners();

    await Future.delayed(const Duration(seconds: 2));
    _user = User("Vishal");

    _loading = false;
    notifyListeners();
  }
}
```
### View (UI)
##### 📁 views/home_view.dart
```
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import '../view_models/user_view_model.dart';

class HomeView extends StatelessWidget {
  const HomeView({super.key});

  @override
  Widget build(BuildContext context) {
    final vm = context.watch<UserViewModel>();

    return Scaffold(
      appBar: AppBar(title: const Text("MVVM Example")),
      body: Center(
        child: vm.loading
            ? const CircularProgressIndicator()
            : Column(
                mainAxisSize: MainAxisSize.min,
                children: [
                  Text(vm.user?.name ?? "No User"),
                  const SizedBox(height: 20),
                  ElevatedButton(
                    onPressed: vm.loadUser,
                    child: const Text("Load User"),
                  ),
                ],
              ),
      ),
    );
  }
}
```
### main.dart
```
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import 'views/home_view.dart';
import 'view_models/user_view_model.dart';

void main() {
  runApp(
    ChangeNotifierProvider(
      create: (_) => UserViewModel(),
      child: const MyApp(),
    ),
  );
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      home: HomeView(),
    );
  }
}
```
---
## 7️⃣ Why This MVVM Setup Is Clean
* ✅ UI has no logic
* ✅ Logic is testable
* ✅ State is centralized
* ✅ Code is scalable
* ✅ Easy to add API later
  
---
### How It Works
1. User clicks a button in the View
2. View calls a method in ViewModel
3. ViewModel updates state
4. UI rebuilds automatically

---
### When to Use MVVM?
* Production apps
* Medium to large Flutter projects
* Team-based development
* Apps that require scalability

---

## 📜 License
MIT License
```
Copyright (c) 2025 Excelsior Technologies

Permission is hereby granted, free of charge, to any person obtaining a copy  
of this software and associated documentation files (the "Software"), to deal  
in the Software without restriction, including without limitation the rights  
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell  
copies of the Software, and to permit persons to whom the Software is  
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all  
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED **"AS IS"**, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR  
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,  
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
```
---

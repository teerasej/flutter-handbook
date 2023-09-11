
# ติดตั้ง **GetX** State Management

## 1. สร้างโปรเจคใหม่

1. ทำการสร้างโปรเจคใหม่ ชื่อ **fast_news**
2. เปิด directory ของโปรเจคขึ้นมาในโปรแกรม Visual Studio Code

## 2. ติดตั้ง GetX 

1. รันคำสั่งด้านล่าง เพื่อติดตั้ง [GetX package](https://pub.dev/packages/get)

```
flutter pub add get
```

2. เปิดไฟล์ `lib/main.dart` เพื่อแก้ไขให้ใช้ `GetMaterialApp` Widget ของ GetX แทนที่ `MaterialApp` ของ Flutter เดิม

```dart
// lib/main.dart

import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {

    // เปลี่ยนจาก MaterialApp เป็น GetMaterialApp
    return GetMaterialApp(
      // เปลี่ยนชื่อ 
      title: 'Fast News',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,
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
          children: <Widget>[
            const Text(
              'You have pushed the button this many times:',
            ),
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




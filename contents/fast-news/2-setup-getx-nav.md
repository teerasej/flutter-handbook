
# ตั้งค่า Navigation System ด้วย `GetPage`

- `getPages` เป็น property ใน `GetMaterialApp` widget ของ get package
- โดยเราจะใส่ list ของ `GetPage` ที่เป็นตัวแทนของหน้า page (บางคนเรียกเป็นหน้าแอพ, Screen)
-  `GetPage` แต่ละตัวจะมี 
   -  **name** property ที่สามารถกำหนด route name (URL Path) 
   -  **page** property ที่กำหนด widget ที่จะแสดงขึ้นมาในแอพ ตอนที่เราสั่งเปิดไปยัง route ดังกล่าว

```dart
// src/main.dart

import 'package:flutter/material.dart';
import 'package:get/route_manager.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return GetMaterialApp(
      title: 'Fast News',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,
      ),
      // กำหนด route name เริ่มต้น เวลาเริ่มแอพ
      initialRoute: '/',
      
      // กำหนด MyHomePage widget ให้มี route name ชื่อ '/'
      getPages: [
        GetPage(name: '/', page: () => const MyHomePage(title: 'Home'))
      ],
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
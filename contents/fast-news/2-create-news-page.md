
# สร้าง News Page ด้วย ListView 

## 1. สร้าง Widget News Page

สร้างไฟล์ `lib/presentations/screens/news/news_page.dart`

```dart
// lib/presentations/screens/news/news_page.dart

import 'package:flutter/material.dart';

class NewsPage extends StatelessWidget {
  const NewsPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('News'),
      ),
      body: ListView(
        children: const [
          ListTile(
            title: Text('Wow'),
            subtitle: Text('news 1'),
          ),
          ListTile(
            title: Text('Ok'),
            subtitle: Text('news 2'),
          )
        ],
      ),
    );
  }
}

```

## 2. เพิ่ม News Page และ Route เข้าไปใน GetMaterialApp 

- `getPages` เป็น property ใน `GetMaterialApp` widget ของ get package
- โดยเราจะใส่ list ของ `GetPage` ที่เป็นตัวแทนของหน้า page (บางคนเรียกเป็นหน้าแอพ, Screen)
-  `GetPage` แต่ละตัวจะมี 
   -  **name** property ที่สามารถกำหนด route name (URL Path) 
   -  **page** property ที่กำหนด widget ที่จะแสดงขึ้นมาในแอพ ตอนที่เราสั่งเปิดไปยัง route ดังกล่าว

```dart
// src/main.dart

import 'package:flutter/material.dart';
import 'package:get/route_manager.dart';
import 'package:fast_news_app/presentations/screens/news/news_page.dart';

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
          appBarTheme: AppBarTheme(
            color: Colors.blue[400],
            foregroundColor: Colors.white,
          )),

      // กำหนด route name เริ่มต้นเป็น '/'
      initialRoute: '/',

      // กำหนด Route name และ NewsPage เข้าไปใน GetMaterialApp
      getPages: [
        GetPage(
          name: '/',
          page: () => const NewsPage(),
        )
      ],
    );
  }
}

// ลบ MyHomePage Widget

```
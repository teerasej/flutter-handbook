
# สร้าง News Detail Page

## 1. สร้าง News Detail Page 

สร้างไฟล์ `lib/presentations/screens/news/news_detail_page.dart` สำหรับการแสดงข่าวที่เลือกจากหน้า News Page 

```dart
// lib/presentations/screens/news/news_detail_page.dart

import 'package:flutter/material.dart';

class NewsDetailPage extends StatelessWidget {
  const NewsDetailPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Headline"),
        actions: [
          IconButton(
            onPressed: () {},
            icon: const Icon(Icons.share),
          ),
        ],
      ),
      body: Text("lorem"),
    );
  }
}

```

## 2. เพิ่ม News Detail Page เข้าไปในแอพ


```dart 
// lib/main.dart

import 'package:flutter/material.dart';
import 'package:get/route_manager.dart';
import 'package:fast_news_app/presentations/screens/news/news_detail_page.dart';
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

      // เปลี่ยน route เริ่มต้นเป็น News page
      initialRoute: '/news',
      getPages: [
        GetPage(
          // เปลี่ยน route name ของ news page ใหม่ เป็น '/news'
          name: '/news',
          page: () => const NewsPage(),
        ),

        // ทำการเพิ่ม News Detail Page และ route name
        GetPage(
          name: '/news/detail',
          page: () => const NewsDetailPage(),
        ),
      ],
    );
  }
}

```

## 3. ทำให้การกดเลือกข่าวจากหน้า News Page เปิดไปหน้า News Detail Page


```dart
// lib/presentations/screens/news/news_page.dart

import 'package:flutter/material.dart';
import 'package:get/route_manager.dart';

class NewsPage extends StatelessWidget {
  const NewsPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('News'),
      ),
      body: ListView(
        children: [
          ListTile(
            title: Text('Wow'),
            subtitle: Text('news 1'),

            // เพิ่ม parameter onTap โดยการใช้ Get.toName ไปยัง URL 
            onTap: () {
              Get.toNamed('/news/detail');
            },
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

# ใช้งาน Future Builder

## 1. สร้าง News page

สร้าง `lib/pages/news_page/news_page.dart`

```dart
// lib/pages/news_page/news_page.dart

import 'package:app_fast_news/controllers/news/news_controller.dart';
import 'package:flutter/material.dart';
import 'package:get/get.dart';

class NewsPage extends StatelessWidget {
  NewsPage({super.key});
  
  // เรียกใช้งาน News Controller
  final NewsController newsController = Get.find<NewsController>();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('News'),
        automaticallyImplyLeading: false,
      ),
      // ใช้งาน FutureBuilder
      body: FutureBuilder(
        // เรียกใช้ loadNews() async method
        future: newsController.loadNews(),
        builder: (BuildContext context, AsyncSnapshot<dynamic> snapshot) {
          
          // ถ้า future ทำงานเสร็จ จะเช็ค done 
          if (snapshot.connectionState == ConnectionState.done) {

            // แสดงข้อความเวลาการทำงานสมบูรณ์
            return Text('News Loaded.');
          }

          // ถ้า future ยังไม่อยู่ในสถานะ done state ให้แสดงตัวโหลด
          return const Center(
            child: CircularProgressIndicator(),
          );
        },
      ),
    );
  }
}

```

## 2. เพิ่ม News page ลงไปใน App 

```dart
// lib/main.dart

import 'package:app_fast_news/pages/news_page/news_page.dart';
import 'package:app_fast_news/pages/sign_in_page/sign_in_page.dart';
import 'package:app_fast_news/pages/sign_up_page/sign_up_page.dart';
import 'package:app_fast_news/pages/state_validator_page/state_validator_page.dart';
import 'package:app_fast_news/utils/dependency_injection.dart';
import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:get/route_manager.dart';
import 'package:get_storage/get_storage.dart';

void main() async {
  await GetStorage.init();
  await DependencyInjection.init();
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
        ),
      ),
      initialRoute: '/',
      getPages: [
        GetPage(
          name: '/',
          page: () => const StateValidatorPage(),
        ),
        GetPage(
          name: '/sign-in',
          page: () => const SignInPage(),
        ),
        GetPage(
          name: '/sign-up',
          page: () => const SignUpPage(),
        ),

        // เพิ่ม News Page ใน App
        GetPage(
          name: '/news',
          page: () => NewsPage(),
        ),
      ],
    );
  }
}

```

## 3. แสดง News page 

```dart
import 'dart:async';

import 'package:after_layout/after_layout.dart';
import 'package:app_fast_news/controllers/auth/auth_controller.dart';
import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:get/route_manager.dart';

class StateValidatorPage extends StatefulWidget {
  const StateValidatorPage({super.key});

  @override
  State<StateValidatorPage> createState() => _StateValidatorPageState();
}

class _StateValidatorPageState extends State<StateValidatorPage>
    with AfterLayoutMixin<StateValidatorPage> {
  final AuthController authController = Get.find<AuthController>();

  @override
  Widget build(BuildContext context) {
    print('rendering the UI');

    return Container(
      color: Colors.white,
      child: const Center(
        child: CircularProgressIndicator(),
      ),
    );
  }

  @override
  FutureOr<void> afterFirstLayout(BuildContext context) {
    print('run after rendered the UI');

    // แก้เงื่อนไขให้เปิดหน้า news จากการที่เช็คแล้วมี token
    if (authController.getToken().isNotEmpty) {
      Get.toNamed('/sign-in');
    } else {
      // เปิดหน้า news page 
      Get.toNamed('/news');
    }
  }
}

```
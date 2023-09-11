
# สร้างหน้า Sign up

## 1. สร้าง Widget เป็นหน้า Sign up

```dart
// lib/pages/sign_up_page/sign_up_page.dart

import 'package:flutter/material.dart';

class SignUpPage extends StatelessWidget {
  const SignUpPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Create Account')),
      body: Padding(
        padding: const EdgeInsets.all(10),
        child: Column(
          children: [
            SizedBox(
              width: double.maxFinite,
              child: ElevatedButton(
                child: const Text('Create'),
                onPressed: () {},
              ),
            ),
            SizedBox(
              width: double.maxFinite,
              child: ElevatedButton(
                child: const Text('Cancel'),
                onPressed: () {},
              ),
            ),
          ],
        ),
      ),
    );
  }
}


```

## 2. เพิ่ม SignUpPage เข้าไปในแอพ

```dart
// lib/main.dart


import 'package:app_fast_news/pages/sign_in_page/sign_in_page.dart';

// import widget
import 'package:app_fast_news/pages/sign_up_page/sign_up_page.dart';
import 'package:app_fast_news/pages/state_validator_page/state_validator_page.dart';
import 'package:flutter/material.dart';
import 'package:get/route_manager.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // This widget is the root of your application.
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
      initialRoute: '/sign-in',
      getPages: [
        GetPage(
          name: '/',
          page: () => const StateValidatorPage(),
        ),
        GetPage(
          name: '/sign-in',
          page: () => const SignInPage(),
        ),

        // เพิ่ม SignUpPage เข้าไปในแอพ
        GetPage(
          name: '/sign-up',
          page: () => const SignUpPage(),
        ),
      ],
    );
  }
}


```
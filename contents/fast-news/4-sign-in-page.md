
# สร้างหน้า Sign in

## 1. สร้าง Widget เป็นหน้า Sign in

```dart
// lib/pages/sign_in_page/sign_in_page.dart

import 'package:flutter/material.dart';

class SignInPage extends StatelessWidget {
  const SignInPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Sign in')),
      body: Padding(
        padding: const EdgeInsets.all(10),
        child: SizedBox(
          width: double.maxFinite,
          child: ElevatedButton(
            child: const Text('Create Account'),
            onPressed: () {},
          ),
        ),
      ),
    );
  }
}

```

## 2. เพิ่ม SignInPage เข้าไปในแอพ

```dart
// lib/main.dart

// import widget
import 'package:app_fast_news/pages/sign_in_page/sign_in_page.dart';
import 'package:app_fast_news/pages/state_validator_page/state_validator_page.dart';
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

      // แก้ให้หน้าแรกเป็น sign in page
      initialRoute: '/sign-in',

      getPages: [
        GetPage(
          name: '/',
          page: () => const StateValidatorPage(),
        ),

        // เพิ่ม sign-in page widget และ route name
        GetPage(
          name: '/sign-in',
          page: () => const SignInPage(),
        ),
      ],
    );
  }
}

```

## 3. กำหนด Theme ให้ App bar

```dart
// lib/main.dart

import 'package:app_fast_news/pages/sign_in_page/sign_in_page.dart';
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

        // กำหนด appBarTheme เป็นพื้นหลังสีฟ้า และตัวหนังสือสีขาว
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
      ],
    );
  }
}

```

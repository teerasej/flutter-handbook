
# สร้างส่วนจัดการ Dependency Injection

## 1. สร้าง class dependency injection 

สร้างไฟล์ `lib/utils/dependency_injection.dart`

```dart
// lib/utils/dependency_injection.dart

import 'dart:async';

import 'package:app_fast_news/controllers/auth/auth_controller.dart';
import 'package:get/get.dart';

class DependencyInjection {
  static FutureOr<void> init() async {
    Get.put<AuthController>(AuthController());
  }
}
```

## 2. ย้าย Get ใน main.dart ไปไว้ใน dependency injection และเรียกใช้งานแทน

```dart
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

  // เรียกใช้งาน DependencyInjection แทน 
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
      ],
    );
  }
}

```
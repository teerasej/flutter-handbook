
# สร้างกลไกเก็บและเรียกดู token

ทั้งหมดนี้มี 8 ขั้นตอน

## 1. ติดตั้ง Package

### 1.1 ติดตั้ง GetStorage

รันคำสั่งติดตั้ง [GetStorage](https://pub.dev/packages/get_storage) ใน Terminal (Command Prompt) ของโปรเจค 

```bash
flutter pub add get_storage
```

ส่วนนี้จะใช่้ในการเก็บและเช็ค token

### 1.2 ติดตั้ง after_layout

รันคำสั่งติดตั้ง [after_layout](https://pub.dev/packages/after_layout) ใน Terminal (Command Prompt) ของโปรเจค 

```bash
flutter pub add after_layout
```

ส่วนนี้ใช้ในการรันโค้ดหลังจากที่ Widget แสดงขึ้นมาแล้ว



### 1.3 ติดตั้ง path_provider

รันคำสั่งติดตั้ง [path_provider](https://pub.dev/packages/path_provider) ใน Terminal (Command Prompt) ของโปรเจค

```bash
flutter pub add path_provider
```

ส่วนนี้มีการใช้ในส่วนของ GetStorage

## 2. เริ่มต้นการทำงานของ GetStorage 

```dart
// lib/main.dart 

import 'package:app_fast_news/pages/sign_in_page/sign_in_page.dart';
import 'package:app_fast_news/pages/sign_up_page/sign_up_page.dart';
import 'package:app_fast_news/pages/state_validator_page/state_validator_page.dart';
import 'package:flutter/material.dart';
import 'package:get/route_manager.dart';

// import get_storage
import 'package:get_storage/get_storage.dart';

// ปรับให้ main method เป็น async 
void main() async {

  // เริ่มต้นการทำงานของ GetStorage    
  await GetStorage.init();

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
        GetPage(
          name: '/sign-up',
          page: () => const SignUpPage(),
        ),
      ],
    );
  }
}

```

## 3. สร้าง Controller สำหรับจัดการ Authentication

เราจะสร้างไฟล์ controller มาไว้ในไฟล์ `lib/controllers/auth/auth_controller.dart`

```dart
// lib/controllers/auth/auth_controller.dart

import 'package:get/get.dart';
import 'package:get_storage/get_storage.dart';

class AuthController extends GetxController {
  
  static final box = GetStorage();
  String token = "";

  // Function สำหรับเก็บ token ไว้ใน GetStorage
  void saveToken(String token) {
    box.write('token', token);
    this.token = token;
  }

  // Function สำหรับการเรียก token ที่เก็บไว้ใน GetStorage
  String getToken() {
    return box.read('token') ?? ''; 
  }

  // Function สำหรับลบ token ออกจาก GetStorage
  void deleteToken() {
    box.remove('token');
    this.token = "";
  }
}

```

## 4. เพิ่ม AuthController เข้าไปใน Get

เราสามารถเพิ่ม Controller เข้าไปในระบบ Get ได้ ซึ่งในที่นี้เราจะทำใน `main()` function 

```dart
// lib/main.dart

// import AuthController
import 'package:app_fast_news/controllers/auth/auth_controller.dart';
import 'package:app_fast_news/pages/sign_in_page/sign_in_page.dart';
import 'package:app_fast_news/pages/sign_up_page/sign_up_page.dart';
import 'package:app_fast_news/pages/state_validator_page/state_validator_page.dart';
import 'package:flutter/material.dart';

// import Get
import 'package:get/get.dart';
import 'package:get/route_manager.dart';
import 'package:get_storage/get_storage.dart';

void main() async {
  await GetStorage.init();
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {

    // สร้าง AuthController และเพิ่มเข้าไปในระบบ Get เพื่อเรียกใช้ต่อไปใน Widget
    Get.put(AuthController());

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
        GetPage(
          name: '/sign-up',
          page: () => const SignUpPage(),
        ),
      ],
    );
  }
}

```

## 5. ปรับให้ route เริ่มต้นเป็น State Validator Page


```dart
// lib/main.dart

import 'package:app_fast_news/controllers/auth/auth_controller.dart';
import 'package:app_fast_news/pages/sign_in_page/sign_in_page.dart';
import 'package:app_fast_news/pages/sign_up_page/sign_up_page.dart';
import 'package:app_fast_news/pages/state_validator_page/state_validator_page.dart';
import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:get/route_manager.dart';
import 'package:get_storage/get_storage.dart';

void main() async {
  await GetStorage.init();
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {

    Get.put(AuthController());

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

      // กำหนดให้ route name เริ่มต้นเป็น State Validator Page
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

## 6. เปลี่ยน StateValidatorPage เป็น Stateful widget เพราะต้องการใช้ after_layout 

```dart
// lib/pages/state_validator_page/state_validator_page.dart

import 'dart:async';

import 'package:after_layout/after_layout.dart';
import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:get/route_manager.dart';

class StateValidatorPage extends StatefulWidget {
  const StateValidatorPage({super.key});

  @override
  State<StateValidatorPage> createState() => _StateValidatorPageState();
}

// ใช้ AfterLayoutMixin เพื่อเสริมการทำงานของ State
class _StateValidatorPageState extends State<StateValidatorPage>
    with AfterLayoutMixin<StateValidatorPage> {

  @override
  Widget build(BuildContext context) {
    return Container(
      color: Colors.white,
      child: const Center(
        child: CircularProgressIndicator(),
      ),
    );
  }

  // method ที่จะทำงานหลังจาก build method ทำงานเสร็จแล้ว
  @override
  FutureOr<void> afterFirstLayout(BuildContext context) {
    print('run after rendered the UI');
  }
}

```

## 7. เรียกใช้งาน AuthController ใน `StateValidatorPage`

```dart
// lib/pages/state_validator_page/state_validator_page.dart

import 'dart:async';

import 'package:after_layout/after_layout.dart';
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

  // เรียกใช้ AuthController จาก Get
  final AuthController authController = Get.find<AuthController>();

  @override
  Widget build(BuildContext context) {
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

    // ถ้าไม่มี token จะเปิดไปยัง Sign in page
    if (authController.getToken().isEmpty) {
      Get.toNamed('/sign-in');
    } else {
      // Get name
    }
  }
}

```

## 8. ปรับให้หน้า Sign in Page ไม่แสดงปุ่ม back บน App Bar

```dart
// lib/pages/sign_in_page/sign_in_page.dart

import 'package:flutter/material.dart';
import 'package:get/route_manager.dart';

class SignInPage extends StatelessWidget {
  const SignInPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Sign in'),

        // ไม่แสดงปุ่ม back ใน App Bar
        automaticallyImplyLeading: false,
      ),
      body: Padding(
        padding: const EdgeInsets.all(10),
        child: SizedBox(
          width: double.maxFinite,
          child: ElevatedButton(
            child: const Text('Create Account'),
            onPressed: () {
              Get.toNamed('/sign-up');
            },
          ),
        ),
      ),
    );
  }
}

```

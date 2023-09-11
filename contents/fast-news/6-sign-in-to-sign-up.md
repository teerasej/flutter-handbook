
# กำหนด Navigation จาก Sign in page ไป Sign up page

## 1. กำหนดให้การกดปุ่มในหน้า Sign in Page ไปหน้า Sign up page

```dart
// lib/pages/sign_in_page/sign_in_page.dart

import 'package:flutter/material.dart';
import 'package:get/route_manager.dart';

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

            // เรียกใช้ Get เพื่อเปิดไป route name '/sign-up' ตามที่เรากำหนดไว้ใน GetMaterialApp
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

## 2. กำหนดให้การกดปุ่ม Cancel ในหน้า Sign up Page กลับไปหน้า Sign in page

```dart
// lib/pages/sign_up_page/sign_up_page.dart

import 'package:flutter/material.dart';
import 'package:get/route_manager.dart';

class SignUpPage extends StatelessWidget {
  const SignUpPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Create Account'),
      ),
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
                
                // กำหนดให้การกดปุ่ม เป็นการย้อนกลับไป navigation
                onPressed: () {
                  Get.back();
                },
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

## 3. ซ่อนปุ่ม back บน AppBar ในหน้า Sign up page

```dart
// lib/pages/sign_up_page/sign_up_page.dart

import 'package:flutter/material.dart';
import 'package:get/route_manager.dart';

class SignUpPage extends StatelessWidget {
  const SignUpPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Create Account'),

        // ยกเลิก leading บน AppBar
        automaticallyImplyLeading: false,

      ),
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
                onPressed: () {
                  Get.back();
                },
              ),
            ),
          ],
        ),
      ),
    );
  }
}

```
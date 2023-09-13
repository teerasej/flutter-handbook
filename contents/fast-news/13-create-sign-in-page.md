
# สร้าง Sign In page และ Sign up page พร้อมแบบฟอร์ม

## 1. สร้าง Sign In Page 

```dart
// lib/presentations/screens/authentication/sign_in_page.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:fast_news_app/presentations/controllers/auth_controller.dart';

class SignInPage extends StatelessWidget {
  const SignInPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Sign in'),
        automaticallyImplyLeading: false,
      ),
      body: Padding(
        padding: const EdgeInsets.all(10),
        child: Form(
          child: Column(
            children: [
              TextFormField(
                decoration: const InputDecoration(
                  labelText: 'Email',
                ),
                keyboardType: TextInputType.emailAddress,
              ),
              TextFormField(
                decoration: const InputDecoration(
                  labelText: 'Password',
                ),
                obscureText: true,
                keyboardType: TextInputType.text,
              ),
              const SizedBox(
                height: 5,
              ),
              SizedBox(
                width: double.maxFinite,
                child: ElevatedButton(
                  child: const Text('Sign in'),
                  onPressed: () {},
                ),
              ),
              SizedBox(
                width: double.maxFinite,
                child: TextButton(
                  child: const Text('Create Account'),
                  onPressed: () {},
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

```

## 2. สร้าง Sign up Page

```dart
// lib/presentations/screens/authentication/sign_up_page.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
class SignUpPage extends StatelessWidget {
  const SignUpPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Create Account'),
        automaticallyImplyLeading: false,
      ),
      body: Padding(
        padding: const EdgeInsets.all(10),
        child: Form(
          child: Column(
            children: [
              TextFormField(
                decoration: const InputDecoration(
                  labelText: 'Email',
                ),
                keyboardType: TextInputType.emailAddress,
              ),
              TextFormField(
                decoration: const InputDecoration(
                  labelText: 'Password',
                ),
                obscureText: true,
                keyboardType: TextInputType.text,
              ),
              const SizedBox(
                height: 5,
              ),
              SizedBox(
                width: double.maxFinite,
                child: ElevatedButton(
                  child: const Text('Create Account'),
                  onPressed: () {
                  },
                ),
              ),
              SizedBox(
                width: double.maxFinite,
                child: TextButton(
                  child: const Text('Cancel'),
                  onPressed: () {
                    Get.back();
                  },
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

```

# สร้าง Sign Up use case

อย่าลืม [setup firebase](18-setup-firebase.md) ก่อนนะ



## 1. สร้าง Sign Up use case

```dart
// lib/domain/usecases/sign_up_user_use_case.dart

import 'package:firebase_auth/firebase_auth.dart';
import 'package:fast_news_app/data/repositories/firebase_repository.dart';

class SignUpUserUseCase {
  final FirebaseRepository _firebaseRepository;

  SignUpUserUseCase(this._firebaseRepository);

  Future<UserCredential> execute(String userEmail, String userPassword) {
    return _firebaseRepository.createUser(userEmail, userPassword);
  }
}

```

## 2. เรียก Sign up use case จากใน Auth controller

```dart
// lib/presentations/controllers/auth_controller.dart

import 'package:firebase_auth/firebase_auth.dart';
import 'package:firebase_core/firebase_core.dart';
import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:fast_news_app/domain/usecases/sign_in_user_use_case.dart';
import 'package:fast_news_app/domain/usecases/sign_up_user_use_case.dart';

class AuthController extends GetxController {
  final formKeySignIn = GlobalKey<FormState>();
  final formKeySignUp = GlobalKey<FormState>();
  String userEmail = '';
  String userPassword = '';

  SignInUserUseCase _signInUseCase;

  // เก็บ sign up use case
  SignUpUserUseCase _signUpUseCase;

  // กำหนดให้ตอนใช้งาน auth controller ต้องกำหนด sign in และ sign up use case
  AuthController(this._signInUseCase, this._signUpUseCase);

  String? emailValidator(String value) {
    if (value.isEmpty || !value.contains('@')) {
      return 'Please enter a valid email address.';
    }
    return null;
  }

  String? passwordValidator(String value) {
    if (value.isEmpty || value.length < 5) {
      return 'Password must be at least 5 characters long.';
    }
    return null;
  }

  void trySignIn() async {
    final isValid = formKeySignIn.currentState!.validate();
    Get.focusScope!.unfocus();

    if (isValid) {
      formKeySignIn.currentState!.save();
      print(userEmail);
      print(userPassword);

      try {
        await _signInUseCase.execute(userEmail, userPassword);
        Get.toNamed('/news');
      } on FirebaseAuthException catch (e) {
        Get.defaultDialog(
          title: "Oops",
          content: Text("Unable to sign in. ${e.code}"),
        );
      }
    }
  }

  void trySignUp() async {
    final isValid = formKeySignUp.currentState!.validate();
    Get.focusScope!.unfocus();

    if (isValid) {
      formKeySignUp.currentState!.save();
      print(userEmail);
      print(userPassword);

      try {
        // เรียกใช้งาน sign up use case
        var user = await _signUpUseCase.execute(userEmail, userPassword);

        // แสดง pop up เพื่อให้กดเปิดไปหน้า /news
        await Get.defaultDialog(
          title: "Welcome",
          confirm: TextButton(
            onPressed: () {
              Get.toNamed('/news');
            },
            child: Text('Take me to the News'),
          ),
          barrierDismissible: false,
        );
      } on FirebaseAuthException catch (e) {
        Get.defaultDialog(
          title: "Oops",
          content: Text("Unable to sign up. ${e.code}"),
        );
      }
    }
  }
}

```

## 4. สร้าง Use case และ firebase repo ให้กับ Auth controller 

```dart
// lib/presentations/screens/authentication/sign_in_page.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:fast_news_app/data/repositories/firebase_repository.dart';
import 'package:fast_news_app/domain/usecases/sign_in_user_use_case.dart';
import 'package:fast_news_app/domain/usecases/sign_up_user_use_case.dart';
import 'package:fast_news_app/presentations/controllers/auth_controller.dart';

class SignInPage extends StatelessWidget {
  const SignInPage({super.key});

  @override
  Widget build(BuildContext context) {
    final authController = Get.put<AuthController>(
      AuthController(
        SignInUserUseCase(
          FirebaseRepository(),
        ),
        // กำหนด Sign up user case, Firebase Repository เพื่อให้ auth controller ใช้งาน
        SignUpUserUseCase(
          FirebaseRepository(),
        ),
      ),
    );

    return Scaffold(
      appBar: AppBar(
        title: const Text('Sign in'),
        automaticallyImplyLeading: false,
      ),
      body: Padding(
        padding: const EdgeInsets.all(10),
        child: Form(
          key: authController.formKeySignIn,
          child: Column(
            children: [
              // Email field
              TextFormField(
                decoration: const InputDecoration(
                  labelText: 'Email',
                ),
                keyboardType: TextInputType.emailAddress,
                validator: (value) {
                  return authController.emailValidator(value!);
                },
                onSaved: (value) {
                  authController.userEmail = value!;
                },
              ),
              TextFormField(
                decoration: const InputDecoration(
                  labelText: 'Password',
                ),
                obscureText: true,
                keyboardType: TextInputType.text,
                validator: (value) {
                  return authController.passwordValidator(value!);
                },
                onSaved: (value) {
                  authController.userPassword = value!;
                },
              ),
              const SizedBox(
                height: 5,
              ),
              SizedBox(
                width: double.maxFinite,
                child: ElevatedButton(
                  child: const Text('Sign in'),
                  onPressed: () {
                    authController.trySignIn();
                  },
                ),
              ),
              SizedBox(
                width: double.maxFinite,
                child: TextButton(
                  child: const Text('Create Account'),
                  onPressed: () {
                    Get.toNamed('/sign-up');
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

## 5. ทดสอบการทำงานของ Sign Up feature

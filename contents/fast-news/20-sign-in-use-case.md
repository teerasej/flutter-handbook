
# สร้าง Sign In use case

อย่าลืม [setup firebase](18-setup-firebase.md) ก่อนนะ

## 1. สร้าง firebase repository


```dart
// lib/data/repositories/firebase_repository.dart

import 'package:firebase_auth/firebase_auth.dart';

class FirebaseRepository {

  // เรียกใช้งาน sign in ของ FirebaseAuth
  Future<UserCredential> signInUser(String email, String password) async {
    return FirebaseAuth.instance.signInWithEmailAndPassword(
      email: email,
      password: password,
    );
  }

  // เรียกใช้งาน sign up หรือ create account ของ FirebaseAuth
  Future<UserCredential> createUser(String email, String password) async {
    return FirebaseAuth.instance.createUserWithEmailAndPassword(
      email: email,
      password: password,
    );
  }
}
```

## 2. สร้าง Sign In use case

```dart
// lib/domain/usecases/sign_in_user_use_case.dart

import 'package:firebase_auth/firebase_auth.dart';
import 'package:fast_news_app/data/repositories/firebase_repository.dart';

class SignInUserUseCase {
  final FirebaseRepository _firebaseRepository;

  SignInUserUseCase(this._firebaseRepository);

  Future<UserCredential> execute(String userEmail, String userPassword) {
    return _firebaseRepository.signInUser(userEmail, userPassword);
  }
}

```

## 3. เรียก Sign in use case จากใน Auth controller

```dart
// lib/presentations/controllers/auth_controller.dart

import 'package:firebase_auth/firebase_auth.dart';
import 'package:firebase_core/firebase_core.dart';
import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:fast_news_app/domain/usecases/sign_in_user_use_case.dart';

class AuthController extends GetxController {
  final formKeySignIn = GlobalKey<FormState>();
  final formKeySignUp = GlobalKey<FormState>();
  String userEmail = '';
  String userPassword = '';

  // สร้าง ตัวแปรเก็บ sign in use case ไว้ เพื่อเรียกใช้
  SignInUserUseCase _signInUseCase;

  // กำหนดให้กำหนด sign in use case ให้กับ Authcontroller ตอนสร้างขึ้นมาใช้งาน
  AuthController(this._signInUseCase);

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
        // เรียกใช้งาน sign in use case
        await _signInUseCase.execute(userEmail, userPassword);

        // ถ้าผ่านเปิดไปใน news
        Get.toNamed('/news');
    
      // ถ้าเกิด exception (สามารถเกิดจากไม่พบ user ในระบบ) 
      } on FirebaseAuthException catch (e) {
        Get.defaultDialog(
          title: "Oops",
          content: Text("Unable to sign in. ${e.code}"),
        );
      }
    }
  }

  void trySignUp() {
    final isValid = formKeySignUp.currentState!.validate();
    Get.focusScope!.unfocus();

    if (isValid) {
      formKeySignUp.currentState!.save();
      print(userEmail);
      print(userPassword);
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
import 'package:fast_news_app/presentations/controllers/auth_controller.dart';

class SignInPage extends StatelessWidget {
  const SignInPage({super.key});

  @override
  Widget build(BuildContext context) {

    final authController = Get.put<AuthController>(
      // กำหนด sign in use case และ firebase repo ให้กับ AuthController เพื่อเรียกใช้งาน
      AuthController(
        SignInUserUseCase(
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

## 5. ทดสอบการทำงานของ Sign in feature
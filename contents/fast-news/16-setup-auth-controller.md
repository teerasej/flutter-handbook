
# นำ AuthController มาใช้งานกับหน้า Sign in และ Sign up page

## 1. นำ AuthController มาใช้งานกับหน้า Sign in page 

```dart
import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:fast_news_app/presentations/controllers/auth_controller.dart';

class SignInPage extends StatelessWidget {
  const SignInPage({super.key});

  @override
  Widget build(BuildContext context) {

    // Setup AuthController เข้าระบบ Get
    final authController = Get.put<AuthController>(AuthController());

    return Scaffold(
      appBar: AppBar(
        title: const Text('Sign in'),
        automaticallyImplyLeading: false,
      ),
      body: Padding(
        padding: const EdgeInsets.all(10),
        child: Form(

          // เชื่อม Global Key เพื่อควบคุมการทำงานของ Form และ TextFormField
          key: authController.formKeySignIn,

          child: Column(
            children: [
              
              TextFormField(
                decoration: const InputDecoration(
                  labelText: 'Email',
                ),
                keyboardType: TextInputType.emailAddress,

                // เรียกใช้ email validator method ของ AuthController เพื่อ validate ข้อความ email
                validator: (value) {
                  return authController.emailValidator(value!);
                },

                // เก็บค่าที่ได้จาก TextFormField ไว้ใน auth controller ตอนสั่ง FormKey.saved()
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

                // เรียกใช้ password validator method ของ AuthController เพื่อ validate ข้อความ password
                validator: (value) {
                  return authController.passwordValidator(value!);
                },

                // เก็บค่าที่ได้จาก TextFormField ไว้ใน auth controller ตอนสั่ง FormKey.saved()
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
                    // เรียกใช้ method ของ authController เพื่อเก็บข้อมูล 
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


## 2. นำ AuthController มาใช้งานกับหน้า Sign up page 


```dart
import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:fast_news_app/presentations/controllers/auth_controller.dart';

class SignUpPage extends StatelessWidget {
  const SignUpPage({super.key});

  @override
  Widget build(BuildContext context) {

    // เรียกใช้ auth controller จาก GetX
    final authController = Get.find<AuthController>();

    return Scaffold(
      appBar: AppBar(
        title: const Text('Create Account'),
        automaticallyImplyLeading: false,
      ),
      body: Padding(
        padding: const EdgeInsets.all(10),
        child: Form(

          // เชื่อม Global Key เพื่อควบคุมการทำงานของ Form และ TextFormField
          key: authController.formKeySignUp,

          child: Column(
            children: [
              TextFormField(
                decoration: const InputDecoration(
                  labelText: 'Email',
                ),
                keyboardType: TextInputType.emailAddress,

                // เรียกใช้ email validator method ของ AuthController เพื่อ validate ข้อความ email
                validator: (value) {
                  return authController.emailValidator(value!);
                },

                // เก็บค่าที่ได้จาก TextFormField ไว้ใน auth controller ตอนสั่ง FormKey.saved()
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
                
                // เรียกใช้ password validator method ของ AuthController เพื่อ validate ข้อความ password
                validator: (value) {
                  return authController.passwordValidator(value!);
                },

                // เก็บค่าที่ได้จาก TextFormField ไว้ใน auth controller ตอนสั่ง FormKey.saved()
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
                  child: const Text('Create Account'),
                  onPressed: () {

                     // เรียกใช้ method ของ authController เพื่อเก็บข้อมูล 
                    authController.trySignUp();
                    
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
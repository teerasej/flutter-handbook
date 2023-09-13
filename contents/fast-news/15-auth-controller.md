
# สร้าง Controller เพื่อใช้งานกับแบบฟอร์มใน Sign in และ Sign up page

```dart
// lib/presentations/controllers/auth_controller.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';

class AuthController extends GetxController {
  // GlobalKey เพื่อเชื่อมกับ Form Widget ของหน้า Sign in page
  final formKeySignIn = GlobalKey<FormState>();

  // GlobalKey เพื่อเชื่อมกับ Form Widget ของหน้า Sign Up page
  final formKeySignUp = GlobalKey<FormState>();

  // ตัวแปรเก็บ email และ password จาก TextFormField ตอนเกิด event onSaved(value)
  String userEmail = '';
  String userPassword = '';

  // validator method สำหรับ TextFormField.validator ส่วน email
  String? emailValidator(String value) {
    if (value.isEmpty || !value.contains('@')) {
      return 'Please enter a valid email address.';
    }
    return null;
  }

  // validator method สำหรับ TextFormField.validator ส่วน password
  String? passwordValidator(String value) {
    if (value.isEmpty || value.length < 5) {
      return 'Password must be at least 5 characters long.';
    }
    return null;
  }

  // method สำหรับปุ่มในหน้า Sign in
  void trySignIn() {

    // ใช้ global key ในการสั่ง validate TextFormField ใน Form
    final isValid = formKeySignIn.currentState!.validate();
    Get.focusScope!.unfocus();

    // ถ้า validate แล้วผ่านหมดทุก field  
    if (isValid) {
      // ทำการสั่ง save ข้อมูลด้วย Global Key
      // จะทำให้ TextFormField.onSaved(value) ทุกตัวทำงาน
      formKeySignIn.currentState!.save();

      print(userEmail);
      print(userPassword);
    }
  }

   // method สำหรับปุ่มในหน้า Sign up
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
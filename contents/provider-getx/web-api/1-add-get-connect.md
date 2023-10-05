
# 

GetX มีส่วนที่ไว้ให้เราเรียกใช้ RESTful Web API ชื่อ `GetConnect` ซึ่งสามารถสร้าง และเพิ่มเข้าไปในระบบได้เหมือนกับ controller ทั่วไป

```dart
// lib/main.dart
import 'package:flutter/material.dart';
import 'package:get/get_connect.dart';
import 'package:get/get.dart';
import 'package:nextflow_flutter_getx_profiles_api/pages/home_page.dart';

void main() {

  // เพิ่ม GetConnect เข้าระบบแบบ lazy put ทำให้ไม่เปลืองทรัพยากรระบบ จนกว่าจะมีการเรียกใช้งาน
  Get.lazyPut(() => GetConnect());

  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {

    // สังเกตว่าเราใช้ GetMaterialApp แทน MaterialApp 
    return GetMaterialApp(
      title: 'Nextflow Profiles',
      theme: ThemeData(
        primaryColor: Colors.blue,
      ),
      home: HomePage(),
    );
  }
}

```
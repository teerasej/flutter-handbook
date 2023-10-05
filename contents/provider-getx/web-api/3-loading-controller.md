
# สร้างกลไกการแสดง และซ่อนตัวโหลด โดยใช้ Obx widget กับตัวแปร Obs

## 1. สร้างตัวแปร loading เพื่อใช้เป็นเงื่อนไขในการเลือกแสดง UI บนหน้า Home page

```dart
// lib/controllers/profile_controller.dart

import 'package:get/get.dart';

class ProfileController extends GetxController {

  // สร้างตัวแปร Observable (obs) เป็นค่า boolean
  var loading = true.obs;

  // สร้าง method ที่ทำงานแบบ async 
  loadDataFromWebAPI() async {

    // กำหนดค่า true ให้ ตัวแปร loading ตรงนี้จะทำให้ Obx widget ที่มีการดึงตัวแปรนี้ไปใช้มีการ rebuild ตัวเองใหม่
    loading(true);

    // delay 3 seconds
    await Future.delayed(Duration(seconds: 3));

    // กำหนดค่า false ให้ ตัวแปร loading ตรงนี้จะทำให้ Obx widget ที่มีการดึงตัวแปรนี้ไปใช้มีการ rebuild ตัวเองใหม่
    loading(false);
  }
}

```

## 2. สร้างเงื่อนไขในการแสดง Loading icon บนหน้าจอ จากตัวแปร obs ของ controller

```dart
// lib/pages/home_page.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';

class HomePage extends StatelessWidget {
  const HomePage({super.key});

  @override
  Widget build(BuildContext context) {

    // ทำการสร้าง ProfileController และใส่ลงใน GetX
    var controller = Get.put(ProfileController());

    // เรียกใช้ method เพื่อเริ่มกระบวนการที่เตรียมไว้ในขั้นตอนที่แล้ว
    controller.loadDataFromWebAPI();

    return Scaffold(
      appBar: AppBar(
        title: Text('Profiles'),
      ),
      // เรียกใช้ Widget Obx เพราะตั้งใจให้ส่วนนี้ของ UI มีการ rebuild ตามการเปลี่ยนแปลงของตัวแปร Obs ใน controller
      body: Obx(
            () {

            // เช็คค่าตัวแปร obs เพื่อแสดงตัวโหลด
            if (controller.loading.value) {
                return Center(
                    child: CircularProgressIndicator(),
                );
            }
        },
    );
  }
}

```
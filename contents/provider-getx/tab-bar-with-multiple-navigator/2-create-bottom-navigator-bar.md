
# สร้าง Bottom Navigator Bar

## 1. สร้าง Bottom Navigator Bar แบบทั่วไป 

```dart
// lib/main_screen.dart

import 'package:flutter/material.dart';
import 'package:flutter_nextflow_navigation_with_getx/controllers/nav/navigation_controller.dart';
import 'package:get/get.dart';
class MainScreen extends StatelessWidget {

  final NavigationController navController = Get.put(NavigationController());

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      
      // สร้าง Bottom Navigator Bar
      bottomNavigationBar: BottomNavigationBar(
          items: const [
            
            // สร้าง bar item สำหรับ Catalog
            BottomNavigationBarItem(
              icon: Icon(Icons.list),
              label: 'Catalog',
            ),

            // สร้าง bar item สำหรับ Setting
            BottomNavigationBarItem(
              icon: Icon(Icons.settings),
              label: 'Settings',
            ),
          ],
          currentIndex: 1,
        ),
    );
  }
}
```

บันทึกไฟล์และรีเฟรชหน้าจอ จะเห็น Bottom Navigator Bar แสดงขึ้นมา

## 2. กำหนดการทำงานที่อิงกับ Navigation Controller

```dart
// lib/controllers/nav/navigation_controller.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';

class NavigationController extends GetxController {

  // กำหนดค่าเริ่มต้นของ currentIndex ให้เป็น 0 เพื่อใช้ในการกำหนดค่า index ใน Bottom Navigator Bar
  var currentIndex = 0.obs;

  // สร้างฟังก์ชัน changePage สำหรับเปลี่ยน tab ใน Bottom Navigator Bar
  void changePage(int index) {
    currentIndex.value = index;
  }
}
```

## 3. นำ Navigation Controller มาใช้งานใน Main Screen

```dart
// lib/main_screen.dart

import 'package:flutter/material.dart';
import 'package:flutter_nextflow_navigation_with_getx/controllers/nav/navigation_controller.dart';
import 'package:get/get.dart';
class MainScreen extends StatelessWidget {

  final NavigationController navController = Get.put(NavigationController());

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      
      // สังเกตว่ามีการนำ Obx widget มาใช้
      bottomNavigationBar: Obx(
        () => BottomNavigationBar(
          items: const [
            BottomNavigationBarItem(
              icon: Icon(Icons.list),
              label: 'Catalog',
            ),
            BottomNavigationBarItem(
              icon: Icon(Icons.settings),
              label: 'Settings',
            ),
          ],
          
          // กำหนด currentIndex ให้เท่ากับ currentIndex ที่อยู่ใน Navigation Controller
          currentIndex: navController.currentIndex.value,

          // กำหนด onTap ให้เรียกใช้ฟังก์ชัน changePage ใน Navigation Controller
          onTap: navController.changePage,
        ),
      ),
    );
  }
}
```

บันทึกไฟล์ และรีเฟรชหน้าจอ จะเห็น Bottom Navigator Bar ที่สามารถเปลี่ยน tab ได้
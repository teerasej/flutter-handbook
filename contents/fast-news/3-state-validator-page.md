
# สร้าง state validator page

- เราจะสร้าง widget ขึ้นมาเป็น **State validator page** 
- ส่วนนี้จะเป็นหน้าแรกที่เปิดเข้ามาในแอพ
- ใช้ในการเช็ค token หรือข้อมูลอื่นๆ เพื่อใช้ในการเลือกว่า การเปิดแอพนี้จะเปิดต่อไปที่ page ไหนในแอพ

## 1. สร้าง widget ใหม่ 

สร้างไฟล์ `lib/pages/state_validator_page/state_validator_page.dart` เป็น Stateful Widget

```dart
// lib/pages/state_validator_page/state_validator_page.dart

import 'package:flutter/material.dart';

class StateValidatorPage extends StatelessWidget {
  const StateValidatorPage({super.key});

  @override
  Widget build(BuildContext context) {

    // กำหนดหน้านี้เป็นตัวกราฟฟิคโหลดดิ้ง
    return Container(
      color: Colors.white,
      child: const Center(
        child: CircularProgressIndicator(),
      ),
    );
  }
}
```



## 2. กำหนดให้เปิดหน้าแรกของแอพ เป็น StateValidatorPage

```dart
// lib/main.dart

// import widget
import 'package:app_fast_news/pages/state_validator_page/state_validator_page.dart';
import 'package:flutter/material.dart';
import 'package:get/route_manager.dart';

void main() {
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
      ),
      initialRoute: '/',
      getPages: [
        GetPage(
          name: '/',

          // แก้ให้หน้าแรกเป็น StateValidatorPage Widget
          page: () => const StateValidatorPage(),
        ),
      ],
    );
  }
}

```
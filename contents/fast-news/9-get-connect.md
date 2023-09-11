
# สร้างกลไกโหลดเนื้อหาจาก Web API

## 1. เริ่มการทำงานของ GetConnect

`GetConnect` เป็นระบบ HTTP Client คล้ายๆ [Dio](https://pub.dev/packages/dio) และ [http](https://pub.dev/packages/http) 

เริ่มต้นการใช้งานจากการ setup ให้กับ GetX โดยในที่นี้เรามี `dependency_injection` ที่ทำเตรียมไว้

```dart
// lib/utils/dependency_injection.dart

import 'dart:async';

import 'package:app_fast_news/controllers/auth/auth_controller.dart';
import 'package:get/get.dart';

class DependencyInjection {
  static FutureOr<void> init() async {

    // เริ่มการทำงานของ GetConnect() 
    Get.put<GetConnect>(GetConnect());

    Get.put<AuthController>(AuthController());
  }
}

```


## 2. สร้าง NewsController

เราจะสร้าง Controller ใหม่อีกตัวเพื่อที่จะจัดการข้อมูลการเปิดดูข่าว

```dart
// lib/controllers/news/news_controller.dart

import 'dart:convert';

import 'package:get/get.dart';

class NewsController extends GetxController {
  
  // เรียกใช้ GetConnect
  final GetConnect connect = Get.find<GetConnect>();

  Future<void> loadNews() async {

    // ส่ง request แบบ get method ไปหา web api
    var response = await connect.get(
        'https://nextflow-azure-function-node-news-api.azurewebsites.net/api/getNews');

    // ถ้า status code success จะแสดงข้อความ
    if (response.statusCode == 200) {
      print('news loaded')
    }
    return;
  }
}

```

## 3. Setup NewsController 

```dart
import 'dart:async';

import 'package:app_fast_news/controllers/auth/auth_controller.dart';
import 'package:app_fast_news/controllers/news/news_controller.dart';
import 'package:get/get.dart';

class DependencyInjection {
  static FutureOr<void> init() async {
    Get.put<GetConnect>(GetConnect());

    // เรียกใช้งาน NewsController
    Get.put<NewsController>(NewsController());
    
    Get.put<AuthController>(AuthController());
  }
}

```
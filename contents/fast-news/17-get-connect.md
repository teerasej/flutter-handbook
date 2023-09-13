
# สร้างกลไกโหลดเนื้อหาจาก Web API

## 1. เริ่มการทำงานของ GetConnect

`GetConnect` เป็นระบบ HTTP Client คล้ายๆ [Dio](https://pub.dev/packages/dio) และ [http](https://pub.dev/packages/http) 


## 2. สร้าง News Repository Web API

เราจะสร้าง News Repository ใหม่ เพื่อที่จะเขียนส่วนของการเรียกใช้ข้อมูลที่ดึงข้อมูลมาจาก Web API จริง

```dart
// lib/data/repositories/news_repository_web_api.dart

import 'package:get/get.dart';
import 'package:fast_news_app/data/repositories/news_repository_abstract.dart';
import 'package:fast_news_app/domain/entities/news_model/news_model.dart';

class NewsRepositoryWebAPI implements NewsRepositoryAbstract {
  final getConnect = Get.find<GetConnect>();

  @override
  Future<NewsModel> getNews() async {
    // ส่ง request แบบ get method ไปหา web api
    var response = await getConnect.get(
        'https://nextflow-azure-function-node-news-api.azurewebsites.net/api/getNews');

    // ถ้า status code success จะแสดงข้อความ
    if (response.statusCode == 200) {
      print('news loaded');

      // ดึง json string มาแปลงเป็น News Model
      var newsModel = NewsModel.fromJson(response.body);
      return newsModel;
    }

    return NewsModel();
  }
}

```

## 3. สลับมาใช้ News repo web api


```dart
// lib/main.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:fast_news_app/data/repositories/news_repository.dart';
import 'package:fast_news_app/data/repositories/news_repository_web_api.dart';
import 'package:fast_news_app/domain/usecases/get_news_use_case.dart';
import 'package:fast_news_app/presentations/controllers/news_controller.dart';
import 'package:fast_news_app/presentations/screens/authentication/sign_in_page.dart';
import 'package:fast_news_app/presentations/screens/authentication/sign_up_page.dart';
import 'package:fast_news_app/presentations/screens/news/news_detail_page.dart';
import 'package:fast_news_app/presentations/screens/news/news_page.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    Get.put(GetConnect());

    Get.put<NewsController>(
      NewsController(
        GetNewsUseCase(
          // สลับจาก News Repo Mockup มาเป็น News Repo Web API
          NewsRepositoryWebAPI(),
        ),
      ),
    );

    return GetMaterialApp(
      title: 'Fast News',
      theme: ThemeData(
          colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
          useMaterial3: true,
          appBarTheme: AppBarTheme(
            color: Colors.blue[400],
            foregroundColor: Colors.white,
          )),
      initialRoute: '/news',
      getPages: [
        GetPage(
          name: '/news',
          page: () => const NewsPage(),
        ),
        GetPage(
          name: '/news/detail',
          page: () => const NewsDetailPage(),
        ),
        GetPage(
          name: '/sign-in',
          page: () => const SignInPage(),
        ),
        GetPage(
          name: '/sign-up',
          page: () => const SignUpPage(),
        ),
      ],
    );
  }
}

```
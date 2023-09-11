
# เปิดดูรายละเอียดข่าว

## 1. ทำให้ controller สามารถเก็บข่าวที่ถูกเลือก 

```dart
// lib/controllers/news/news_controller.dart
import 'dart:convert';

import 'package:app_fast_news/models/news_model/news.dart';
import 'package:app_fast_news/models/news_model/news_model.dart';
import 'package:get/get.dart';

class NewsController extends GetxController {
  final GetConnect connect = Get.find<GetConnect>();

  var newsModel = NewsModel();

  // สร้าง property สำหรับเก็บข่่าวที่ถูกเลือก
  News? selectedNews;

  Future<void> loadNews() async {
    var response = await connect.get(
        'https://nextflow-azure-function-node-news-api.azurewebsites.net/api/getNews');
    if (response.statusCode == 200) {
      newsModel = NewsModel.fromJson(jsonDecode(response.body));
    }
    return;
  }
}

```

## 2. สร้างหน้า NewsDetailPage

```dart
// lib/pages/news_page/news_detail_page.dart

import 'package:app_fast_news/controllers/news/news_controller.dart';
import 'package:flutter/material.dart';
import 'package:get/get.dart';

class NewsDetailPage extends StatelessWidget {
  NewsDetailPage({super.key});

  // เรียกใช้ controller 
  final NewsController newsController = Get.find<NewsController>();

  @override
  Widget build(BuildContext context) {

    // เรียกใช้งาน selected news
    var selectedNews = newsController.selectedNews;

    return Scaffold(
      appBar: AppBar(
        title: Text("${selectedNews?.headline}"),
        actions: [
          IconButton(
            onPressed: () {},
            icon: const Icon(Icons.open_in_browser),
          ),
        ],
      ),
      body: Text("${selectedNews?.content}"),
    );
  }
}

```

## 3. เพิ่ม News detail page ในแอพ

```dart
// lib/main.dart

import 'package:app_fast_news/pages/news_page/news_detail_page.dart';
import 'package:app_fast_news/pages/news_page/news_page.dart';
import 'package:app_fast_news/pages/sign_in_page/sign_in_page.dart';
import 'package:app_fast_news/pages/sign_up_page/sign_up_page.dart';
import 'package:app_fast_news/pages/state_validator_page/state_validator_page.dart';
import 'package:app_fast_news/utils/dependency_injection.dart';
import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:get/route_manager.dart';
import 'package:get_storage/get_storage.dart';

void main() async {
  await GetStorage.init();
  await DependencyInjection.init();
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
        appBarTheme: AppBarTheme(
          color: Colors.blue[400],
          foregroundColor: Colors.white,
        ),
      ),
      initialRoute: '/',
      getPages: [
        GetPage(
          name: '/',
          page: () => const StateValidatorPage(),
        ),
        GetPage(
          name: '/sign-in',
          page: () => const SignInPage(),
        ),
        GetPage(
          name: '/sign-up',
          page: () => const SignUpPage(),
        ),
        GetPage(
          name: '/news',
          page: () => NewsPage(),
        ),

        // เพิ่ม News Detail
        GetPage(
          name: '/news/detail',
          page: () => NewsDetailPage(),
        ),
      ],
    );
  }
}

```

## 4. เก็บข่าวไว้ใน controller และเปิดหน้า news/detail

```dart
// lib/pages/news_page/news_page.dart

import 'package:app_fast_news/controllers/news/news_controller.dart';
import 'package:flutter/material.dart';
import 'package:get/get.dart';

class NewsPage extends StatelessWidget {
  NewsPage({super.key});

  final NewsController newsController = Get.find<NewsController>();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('News'),
        automaticallyImplyLeading: false,
      ),
      body: FutureBuilder(
        future: newsController.loadNews(),
        builder: (BuildContext context, AsyncSnapshot<dynamic> snapshot) {
          if (snapshot.connectionState == ConnectionState.done) {
            var newsList = newsController.newsModel.news;

            return ListView.builder(
              itemCount: newsList?.length ?? 0,
              itemBuilder: (BuildContext context, int index) {
                var news = newsList?[index];
                return ListTile(
                  title: Text(news?.headline ?? '...'),
                  onTap: () {

                    // เก็บข่าวที่เลือกจาก Widget ไว้ใน controller
                    newsController.selectedNews = news;

                    // เปิดหน้า news detail
                    Get.toNamed('/news/detail');
                  },
                );
              },
            );
          }

          return const Center(
            child: CircularProgressIndicator(),
          );
        },
      ),
    );
  }
}
```
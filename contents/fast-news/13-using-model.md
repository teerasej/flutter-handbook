
# ใช้งาน Model class

## 1. แปลง JSON เป็น NewsModel เพื่อใช้งานในแอพ

เปิด `lib/controllers/news/news_controller.dart`

```dart
import 'dart:convert';

import 'package:app_fast_news/models/news_model/news.dart';
import 'package:app_fast_news/models/news_model/news_model.dart';
import 'package:get/get.dart';

class NewsController extends GetxController {
  final GetConnect connect = Get.find<GetConnect>();

  var newsModel = NewsModel();

  Future<void> loadNews() async {
    var response = await connect.get(
        'https://nextflow-azure-function-node-news-api.azurewebsites.net/api/getNews');
    if (response.statusCode == 200) {

      // แปลง JSON ที่ได้จาก Web API ด้วย fromJson method 
      newsModel = NewsModel.fromJson(jsonDecode(response.body));
    }
    return;
  }
}

```


## 2. ดึงข้อมูลข่าวจาก NewsController มาใช้ใน FutureBuilder

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

            // ดึึง list จาก newsModel จาก controller
            var newsList = newsController.newsModel.news;

            // สร้าง ListView ด้วย .builder()
            return ListView.builder(
              itemCount: newsList?.length ?? 0,
              itemBuilder: (BuildContext context, int index) {

                var news = newsList[index];

                return ListTile(
                  title: Text(news?.headline ?? '...'),
                  onTap: () {
                    
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
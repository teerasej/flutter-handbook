
# นำ News Controller มาเชื่อมการทำงานบน News Page

## 1. นำ Controller มาใช้งานใน News Page

```dart
// lib/presentations/screens/news/news_page.dart

import 'package:flutter/material.dart';

// import ส่วนประกอบจากส่วนต่างๆ มารวมเป็น controller ที่ทำงานได้
import 'package:fast_news_app/data/repositories/news_repository.dart';
import 'package:fast_news_app/domain/usecases/get_news_use_case.dart';
import 'package:fast_news_app/presentations/controllers/news_controller.dart';

import 'package:get/get.dart';

class NewsPage extends StatelessWidget {
  const NewsPage({super.key});

  @override
  Widget build(BuildContext context) {

    // ทำการสร้าง NewsController เก็บไว้ใน GetX
    final newsController = Get.put<NewsController>(
        NewsController(
          GetNewsUseCase(
            NewsRepositoryMockup()
          ),
        ),
      );

    // เรียกใช้ loadNews() เพื่อ set การทำงานของ state
    newsController.loadNews();

    return Scaffold(
      appBar: AppBar(
        title: const Text('News'),
      ),
      // เรียกใช้ Obx widget ที่จะทำงานใหม่ทุกครั้งที่ตัวแปร obs(erable) ที่เรียกใช้ในนี้ มีการอัพเดตค่าใหม่
      body: Obx(
        () {
          // เช็คค่า isLoading เพื่อแสดง/ซ่อน ตัว loading
          if (newsController.isLoading.value) {
            return const Center(
              child: CircularProgressIndicator(),
            );
          } else {

            // ถ้ามีข้อมูล isLoading เป็น false ถือว่ามีข้อมูลใน controller.newsModel ที่เอามาใช้ได้ 
            // เลยเอามาใช้กับ ListView
            return ListView.builder(
              itemCount: newsController.newsModel.value.news?.length ?? 0,
              itemBuilder: (context, index) {

                // ระหว่างการวนเลข index ก็เอา index ที่ได้มาดึงค่าจาก newsModel.news มาใช้สร้าง widget ของ news item แต่ละตัว
                final article = newsController.newsModel.value.news?[index];
                return ListTile(
                  title: Text(article?.headline ?? '...'),
                  subtitle: Text(article?.content ?? '...'),
                );
              },
            );
          }
        },
      ),
    );
  }
}

```

## 2. ทดสอบใช้ Future delay ข้อมูลจาก data source ที่จะส่งกลับไปให้ controller 

ทดสอบหน่วงเวลา

```dart
// lib/data/repositories/news_repository.dart
import 'package:fast_news_app/data/repositories/news_repository_abstract.dart';
import 'package:fast_news_app/domain/entities/news_model/news.dart';
import 'package:fast_news_app/domain/entities/news_model/news_model.dart';


class NewsRepositoryMockUp implements NewsRepositoryAbstract {

  @override
  Future<NewsModel> getNews() async {

     // สร้าง กลไกดีเลย์เวลา 4 วิ ก่อนทำงานต่อ
    await Future.delayed(const Duration(seconds: 4));

    return NewsModel(news: [
      News(
        headline: 'Headline 1',
        link: 'https://www.google.com',
        content: 'Content 1',
      ),
      News(
        headline: 'Headline 2',
        link: 'https://www.google.com',
        content: 'Content 2',
      ),
    ]);
  }
}
```

# เปิดดูรายละเอียดข่าว

## 1. ทำให้ controller สามารถเก็บข่าวที่ถูกเลือก 

```dart
// lib/presentations/screens/news/news_page.dart
import 'package:flutter/material.dart';
import 'package:fast_news_app/data/datasources/news_data_source.dart';
import 'package:fast_news_app/data/repositories/news_repository.dart';
import 'package:fast_news_app/domain/usecases/get_news_use_case.dart';
import 'package:fast_news_app/presentations/controllers/news_controller.dart';
import 'package:get/get.dart';

class NewsPage extends StatelessWidget {
  const NewsPage({super.key});

  @override
  Widget build(BuildContext context) {
    final newsController = Get.put<NewsController>(
        NewsController(GetNewsUseCase(NewsRepository(NewsDataSource()))));

    newsController.loadNews();

    return Scaffold(
      appBar: AppBar(
        title: const Text('News'),
      ),
      body: Obx(
        () {
          if (newsController.isLoading.value) {
            return const Center(
              child: CircularProgressIndicator(),
            );
          } else {
            return ListView.builder(
              itemCount: newsController.newsModel.value.news?.length ?? 0,
              itemBuilder: (context, index) {
                final article = newsController.newsModel.value.news?[index];
                return ListTile(
                  title: Text(article?.headline ?? '...'),
                  subtitle: Text(article?.content ?? '...'),

                  // เพิ่ม action เปิดไปยังหน้า news detail โดยส่ง article object ไปเป็น arguments
                  onTap: () {
                    Get.toNamed('/news/detail', arguments: article);
                  },
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

## 2. เรียกใช้งาน Argument จาก GetX ในหน้า NewsDetailPage

```dart
// lib/presentations/screens/news/news_detail_page.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:fast_news_app/domain/entities/news_model/news.dart';

class NewsDetailPage extends StatelessWidget {
  const NewsDetailPage({super.key});

  @override
  Widget build(BuildContext context) {

    // ดึง Arguments มาจาก News 
    final article = Get.arguments as News;

    return Scaffold(
      appBar: AppBar(
        // แสดงข้อมูลข่าว
        title: Text("${article?.headline ?? '...'}"),
        actions: [
          IconButton(
            onPressed: () {},
            icon: const Icon(Icons.share),
          ),
        ],
      ),
      // แสดงข้อมูลข่าว
      body: Text("${article?.content ?? '...'}"),
    );
  }
}

```

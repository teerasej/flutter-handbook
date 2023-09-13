
# ย้ายส่วนของการสร้าง Controller ไปไว้ใน Main

## 1. Put controller เข้าระบบ Get จากด้านนอกสุด

เราสามารถย้ายการประกาศ Controller ไปที่ `lib/main.dart` ได้ ในที่นี้ Controller จะถูกสร้างตั้งแต่ตอนเริ่มการทำงานของแอพ และสามารถเรียกใช้งานได้ใน Widget ต่างๆ ได้เลย

```dart
// lib/main.dart
import 'package:flutter/material.dart';

// import get.dart
import 'package:get/get.dart';

// import ส่วนประกอบของแต่ละ layer
import 'package:fast_news_app/data/datasources/news_data_source.dart';
import 'package:fast_news_app/data/repositories/news_repository.dart';
import 'package:fast_news_app/domain/usecases/get_news_use_case.dart';
import 'package:fast_news_app/presentations/controllers/news_controller.dart';

import 'package:fast_news_app/presentations/screens/news/news_detail_page.dart';
import 'package:fast_news_app/presentations/screens/news/news_page.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {

    // เพิ่ม News Controller เข้าระบบ Get
    Get.put<NewsController>(
      NewsController(GetNewsUseCase(NewsRepository(NewsDataSource()))),
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
      ],
    );
  }
}

```

## 2. ปรับให้ News Page เรียกใช้ NewsController แทน

```dart
import 'package:flutter/material.dart';

// ลบ import ที่ไม่จำเป็นออก

import 'package:fast_news_app/presentations/controllers/news_controller.dart';
import 'package:get/get.dart';

class NewsPage extends StatelessWidget {
  const NewsPage({super.key});

  @override
  Widget build(BuildContext context) {

    // ดึง News controller ออกมาจาก Get มาใช้แทน
    final newsController = Get.find<NewsController>();

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
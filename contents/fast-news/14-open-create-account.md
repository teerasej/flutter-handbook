
# เปิดหน้า Sign up page จากหน้า Sign in

## 1. เพิ่ม Sign in page และ sign up page เข้าไปในแอพ

```dart
import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:fast_news_app/data/datasources/news_data_source.dart';
import 'package:fast_news_app/data/repositories/news_repository.dart';
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
      initialRoute: '/sign-in',
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

## 2. ทำให้เปิดจากหน้า Sign in ไป Sign up page ได้



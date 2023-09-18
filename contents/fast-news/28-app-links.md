
# 

## 1. ติดตั้ง [app_links](https://pub.dev/packages/app_links) package

รันคำสั่งใน Terminal ภายใน directory ของโปรเจค

```dart
flutter pub add app_links
```


## 2. สร้าง AppLinkController ที่จะทำการจับ link ที่กด active จากแอพหรือเว็บอื่น

สร้างไฟล์ `lib/presentations/controllers/app_link_controller.dart`

```dart
// lib/presentations/controllers/app_link_controller.dart

import 'dart:async';

import 'package:app_links/app_links.dart';
import 'package:get/get.dart';

class AppLinkController extends GetxController {

  // Stream สำหรับใช้งานกับ AppLinks
  StreamSubscription<Uri>? _linkSubscription;
  late AppLinks _appLinks;

  AppLinkController() {
    initDeepLinks();
  }

  // Setup การทำงานของ AppLinks
  Future<void> initDeepLinks() async {
    _appLinks = AppLinks();

    // เช็ค link กับแอพที่อยู่ในสถานะ cold state (terminated)
    final appLink = await _appLinks.getInitialAppLink();
    if (appLink != null) {
      print('getInitialAppLink: $appLink');
      openAppLink(appLink);
    }

    // รองรับการทำงานของ link ในแอพที่อยู่ใน warm state (เปิดอยู่ หรือทำงานอยู่ใน background)
    _linkSubscription = _appLinks.uriLinkStream.listen((uri) {
      print('onAppLink: $uri');
      openAppLink(uri);
    });
  }

  void openAppLink(Uri uri) {

    print(uri.path);

    // เรียกใช้ Get เพื่อเปิด path ที่ได้จาก Uri 
    // Get.offAllNamed(uri.path);
  }

  // ทำลาย Stream instance ทิ้ง เพราะไม่ใช้แล้ว จะได้ไม่ค้างอยู่ในระบบ
  @override
  void dispose() {
    _linkSubscription?.cancel();

    super.dispose();
  }
}
```

## 4. เริ่มการทำงานของ AppLinkController ตอนเปิดแอพ

```dart
// lib/main.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:fast_news_app/data/repositories/firebase_repository.dart';
import 'package:fast_news_app/data/repositories/news_repository_web_api.dart';
import 'package:fast_news_app/domain/usecases/get_news_use_case.dart';
import 'package:fast_news_app/domain/usecases/sign_out_user_use_case.dart';
import 'package:fast_news_app/presentations/controllers/app_link_controller.dart';

import 'package:fast_news_app/presentations/controllers/news_controller.dart';
import 'package:fast_news_app/presentations/screens/authentication/sign_in_page.dart';
import 'package:fast_news_app/presentations/screens/authentication/sign_up_page.dart';
import 'package:fast_news_app/presentations/screens/news/news_detail_page.dart';
import 'package:fast_news_app/presentations/screens/news/news_page.dart';

import 'package:firebase_core/firebase_core.dart';
import 'package:fast_news_app/presentations/screens/utillity/app_validate_page.dart';
import 'firebase_options.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp(
    options: DefaultFirebaseOptions.currentPlatform,
  );
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
          NewsRepositoryWebAPI(),
        ),
        SignOutUserUseCase(
          FirebaseRepository(),
        ),
      ),
    );

    // เริ่มการทำงานของ AppLinkController
    Get.put<AppLinkController>(
      AppLinkController(),
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
      initialRoute: '/',
      getPages: [
        GetPage(
          name: '/news',
          page: () => const NewsPage(),
        ),
        GetPage(
          // ปรับให้ news/detail/ รับค่า URL ส่วนท้ายเป็น parameter ชื่อ newsId
          name: '/news/detail/:newsId',
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
        GetPage(
          name: '/',
          page: () => const AppValidatePage(),
        ),
      ],
    );
  }
}

```

## ทดสอบ

[คลิกเปิดเว็บนี้ในมือถือที่ติดตั้งแอพ](https://ashy-forest-0a270cf10.3.azurestaticapps.net/) และทดสอบการทำงาน
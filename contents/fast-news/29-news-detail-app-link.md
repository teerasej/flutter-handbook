
# ปรับให้ News Detail ตอบสนองต่อการเปิด AppLink

## 1. สร้าง NewsModel Entity ใหม่

ให้แน่ใจว่าได้ใช้ plugin JSONtoDart ของ VSCode สร้าง `NewsModel` และ `News` class ตรงกับ JSON ล่าสุด

1. ลบไฟล์ใน `lib/domain/entities/news_model`
2. copy JSON ตัวอย่างจาก https://nextflow-azure-function-node-news-api.azurewebsites.net/api/getNews 
3. ใช้คำสั่ง JSONtoDart:From Clipboard ในการสร้าง class ชื่อ NewsModel ใหม่
4. เช็คให้แน่ใจว่า News class ที่สร้างใหม่ มี 4 properties

```dart
class News {
  int? id;
  String? headline;
  String? content;
  String? link;
  ...
}
```

## 2. ปรับ Route ของ NewsDetailPage

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

## 3. ปรับ News Repository Abstract ให้มี method ที่ค้นหาข่าวด้วย newsId ได้

```dart
// lib/data/repositories/news_repository_abstract.dart
import 'package:fast_news_app/domain/entities/news_model/news_model.dart';

abstract class NewsRepositoryAbstract {
  Future<NewsModel> getNews();

  // เพิ่ม method สำหรับค้นหาข่าวด้วย id 
  Future<NewsModel> getNewsDetailWithId(int id);
}
```

ซึ่งจะทำให้เราต้อง implement method ดังกล่าวใน NewRepositoryMockUp 

```dart
import 'package:fast_news_app/data/repositories/news_repository_abstract.dart';
import 'package:fast_news_app/domain/entities/news_model/news.dart';
import 'package:fast_news_app/domain/entities/news_model/news_model.dart';

class NewsRepositoryMockUp implements NewsRepositoryAbstract {
  @override
  Future<NewsModel> getNews() async {
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

  // Implement เพิ่ม แต่... ไม่ทำไส้ใน
  @override
  Future<NewsModel> getNewsDetailWithId(int id) {
    // TODO: implement getNewsDetail
    throw UnimplementedError();
  }
}

```

## 4. override `getNewsDetailWithId` ใน News Repository Web API 

```dart
// lib/data/repositories/news_repository_web_api.dart

import 'package:get/get.dart';
import 'package:fast_news_app/data/repositories/news_repository_abstract.dart';
import 'package:fast_news_app/domain/entities/news_model/news.dart';
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
      var newsModel = NewsModel.fromJson(response.body);
      return newsModel;
    }

    return NewsModel();
  }
  
  // override method ตามที่เพิ่มเข้ามาใน abstract class
  @override
  Future<NewsModel> getNewsDetailWithId(int id) async {
    
    // สร้าง JSON object ที่มี key ชื่อ newsId
    var data = {'newsId': id};

    // ส่ง POST request ไปที่ Web API พร้อมด้วย JSON
    var response = await getConnect.post(
      'https://nextflow-azure-function-node-news-api.azurewebsites.net/api/searchNewsWithID',
      data,
    );

    // ถ้า status code 200 ก็จะแปลงข้อมูลที่ได้เป็น instance ของ NewsModel
    if (response.statusCode == 200) {
      print('news loaded');
      var newsModel = NewsModel.fromJson(response.body);
      return newsModel;
    }

    return NewsModel();
  }
}
```

## 5. สร้าง Search News By ID Use case

สร้างไฟล์ `lib/domain/usecases/search_news_use_case.dart`

```dart
// lib/domain/usecases/search_news_use_case.dart

import 'package:fast_news_app/data/repositories/news_repository_abstract.dart';
import 'package:fast_news_app/domain/entities/news_model/news_model.dart';

class SearchNewsUseCase {
  final NewsRepositoryAbstract _repository;

  SearchNewsUseCase(this._repository);

  Future<NewsModel> execute(int id) {
    return _repository.getNewsDetailWithId(id);
  }
}

```

## 6. สร้าง Controller เฉพาะของหน้า NewsDetailPage

- ใช้ในการค้นหา และแสดงข่าวโดยอาศัย newsId ที่ได้จาก URL parameter

สร้างไฟล์ `lib/presentations/controllers/news_detail_controller.dart`

```dart
// lib/presentations/controllers/news_detail_controller.dart
// presentation/controllers/post_controller.dart

import 'package:get/get.dart';
import 'package:fast_news_app/domain/entities/news_model/news.dart';
import 'package:fast_news_app/domain/entities/news_model/news_model.dart';
import 'package:fast_news_app/domain/usecases/search_news_use_case.dart';

class NewsDetailController extends GetxController {
  final SearchNewsUseCase _searchNewsUseCase;

  var newsModel = NewsModel().obs;
  var isLoading = true.obs;

  NewsDetailController(this._searchNewsUseCase);

  Future<void> loadNews() async {
    isLoading(true);

    if (Get.arguments is News) {
      newsModel(
        NewsModel(
          news: [
            Get.arguments as News,
          ],
        ),
      );
    } else if (Get.parameters["newsId"] != null) {
      var newsId = int.parse(Get.parameters["newsId"] ?? "0");
      final response = await _searchNewsUseCase.execute(newsId);
      newsModel(response);
    }

    isLoading(false);
  }
}

```

## 7. กำหนดใช้ controller ในหน้า NewsDetailPage

```dart
// lib/presentations/screens/news/news_detail_page.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:fast_news_app/data/repositories/news_repository_web_api.dart';
import 'package:fast_news_app/domain/entities/news_model/news.dart';
import 'package:fast_news_app/domain/usecases/search_news_use_case.dart';
import 'package:fast_news_app/presentations/controllers/news_controller.dart';
import 'package:fast_news_app/presentations/controllers/news_detail_controller.dart';

class NewsDetailPage extends StatelessWidget {
  const NewsDetailPage({super.key});

  @override
  Widget build(BuildContext context) {
    // สร้างและกำหนดใช้งาน controller
    final controller = Get.put<NewsDetailController>(
      NewsDetailController(
        // กำหนด Search News Use case
        SearchNewsUseCase(
          NewsRepositoryWebAPI(),
        ),
      ),
    );

    // สั่งโหลดข่าว (ในที่นี้คือการค้นหาด้วย newsId จาก Get.parameters)
    controller.loadNews();

    return Scaffold(
      appBar: AppBar(
        // ใช้ Obx ในการแสดงหัวข่าว
        title: Obx(
          () {
            final news = controller.newsModel.value.news;

            if (news == null) {
              return Container();
            }

            if (news.isEmpty) {
              return Text('...');
            } else {
              return Text(news[0].headline ?? '...');
            }
          },
        ),
        actions: [
          // ใช้ Obx ในการแสดงปุ่มแชร์
          Obx(
            () {
              final news = controller.newsModel.value.news;

              if (news == null) {
                return Container();
              }

              if (news.isEmpty) {
                return Container();
              } else {
                return IconButton(
                  onPressed: () {},
                  icon: const Icon(Icons.share),
                );
              }
            },
          ),
        ],
      ),
      // ใช้ Obx ในการแสดงข้อมูลข่าวที่ค้นหาได้จาก net
      body: Obx(() {
        final news = controller.newsModel.value.news;

        if (news == null) {
          return Container();
        }

        if (news.isEmpty) {
          return Center(
            child: CircularProgressIndicator(),
          );
        } else {
          return Padding(
            padding: const EdgeInsets.all(8),
            child: Text("${news[0].content}"),
          );
        }
      }),
    );
  }
}

```

## 8. ปรับให้ NewsPage เปิดหน้า NewsDetailPage โดยการส่ง newsID เป็น URL parameter

```dart
// lib/presentations/screens/news/news_page.dart

import 'package:flutter/material.dart';
import 'package:fast_news_app/presentations/controllers/news_controller.dart';
import 'package:get/get.dart';

class NewsPage extends StatelessWidget {
  const NewsPage({super.key});

  @override
  Widget build(BuildContext context) {
    final newsController = Get.find<NewsController>();

    newsController.loadNews();

    return Scaffold(
      appBar: AppBar(
        title: const Text('News'),
        automaticallyImplyLeading: false,
        actions: [
          IconButton(
            onPressed: () {
              newsController.signOut();
            },
            icon: const Icon(Icons.logout),
          ),
        ],
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
                    Get.toNamed(
                    
                      // ปรับให้ส่ง id ของ news เป็น parameter
                      '/news/detail/${article?.id}',
                    );
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
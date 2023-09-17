
# สร้าง และเรียกใช้ Sign out Use case

ในที่นี้เราจะทำให้หน้า News Page สามารถ sign out ผู้ใช้ออกจากระบบได้ครับ

## 1. เพิ่มกลไกการ sign out user ใน Firebase Repository

เปิดไฟล์ `lib/data/repositories/firebase_repository.dart`

```dart
// lib/data/repositories/firebase_repository.dart
import 'package:firebase_auth/firebase_auth.dart';

class FirebaseRepository {
  Future<UserCredential> signInUser(String email, String password) async {
    return FirebaseAuth.instance.signInWithEmailAndPassword(
      email: email,
      password: password,
    );
  }

  Future<UserCredential> createUser(String email, String password) async {
    return FirebaseAuth.instance.createUserWithEmailAndPassword(
      email: email,
      password: password,
    );
  }

  Future<bool> isUserSignIn() async {
    return FirebaseAuth.instance.currentUser != null;
  }

  // เพิ่มการ Sign out user 
  Future<void> signOutUser() async {
    return FirebaseAuth.instance.signOut();
  }
}

```

## 2. สร้าง Sign out use case

สร้างไฟล์ `lib/domain/usecases/sign_out_user_use_case.dart`

```dart
import 'package:fast_news_app/data/repositories/firebase_repository.dart';

class SignOutUserUseCase {

  // กำหนดให้ use case นี้ต้องมีการประกาศใช้ _firebaseRepository 
  final FirebaseRepository _firebaseRepository;
  SignOutUserUseCase(this._firebaseRepository);
  
  // สั่ง sign out user
  Future<void> execute() {
    return _firebaseRepository.signOutUser();
  }
}

```

## 3. เพิ่ม `SignOutUserUseCase` ใน NewsController 

เปิดไฟล์ `lib/presentations/controllers/news_controller.dart`


```dart
// lib/presentations/controllers/news_controller.dart

// presentation/controllers/post_controller.dart
import 'package:get/get.dart';
import 'package:fast_news_app/domain/entities/news_model/news_model.dart';
import 'package:fast_news_app/domain/usecases/get_news_use_case.dart';
import 'package:fast_news_app/domain/usecases/sign_out_user_use_case.dart';

class NewsController extends GetxController {
  final GetNewsUseCase getNewsUsecase;

  // กำหนดให้การใช้ controller สามารถเรียกใช้ SignOutUserUseCase ได้
  final SignOutUserUseCase signOutUserUseCase;
  
  var newsModel = NewsModel().obs;
  var isLoading = true.obs;
  
  // กำหนดให้การใช้ controller ต้องมีการสร้าง SignOutUserUseCase
  NewsController(this.getNewsUsecase, this.signOutUserUseCase);

  Future<void> loadNews() async {
    isLoading(true);
    final response = await getNewsUsecase.execute();
    newsModel(response);
    isLoading(false);
  }
  
  // เรียกใช้ Use case และ pop up dialog
  Future<void> signOut() async {
    await signOutUserUseCase.execute();
    Get.defaultDialog(
      title: 'Sign Out',
      middleText: 'Sign Out Success',
      barrierDismissible: false,
      onConfirm: () {

        // ถ้ากดปุ่ม ก็จะเปิดไปยังหน้า App Validate Page
        Get.offAllNamed('/');
      },
    );
  }
}

```

## 4. เพิ่มปุ่ม Sign out ในหน้า News Page

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

        // กำหนดให้ news page ไม่แสดงปุ่ม back 
        automaticallyImplyLeading: false,

        // สร้างปุ่มที่เรียกใช้งาน controller ให้ sign out user ออก
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

## 5. เพิ่ม Use case ให้กับ Controller

```dart
// lib/main.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:fast_news_app/data/repositories/firebase_repository.dart';
import 'package:fast_news_app/data/repositories/news_repository_web_api.dart';
import 'package:fast_news_app/domain/usecases/get_news_use_case.dart';
import 'package:fast_news_app/domain/usecases/sign_out_user_use_case.dart';

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
        // เพิ่ม SignOutUserUseCase และ FirebaseRepository ของมัน
        SignOutUserUseCase(
          FirebaseRepository(),
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
      initialRoute: '/',
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
        GetPage(
          name: '/',
          page: () => const AppValidatePage(),
        ),
      ],
    );
  }
}

```